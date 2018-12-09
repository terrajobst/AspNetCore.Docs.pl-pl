---
title: Ustawienia zewnętrznej nazwy logowania usługi Facebook w programie ASP.NET Core
author: rick-anderson
description: Ten samouczek przedstawia integracja uwierzytelniania użytkownika konta serwisu Facebook do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/facebook-logins
ms.openlocfilehash: 8bb22dc6df9879e827ff9a5ac11e9e3ad5346dc2
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121508"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="02279-103">Ustawienia zewnętrznej nazwy logowania usługi Facebook w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="02279-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="02279-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="02279-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="02279-105">W tym samouczku dowiesz się, jak umożliwić użytkownikom logowanie się przy użyciu swojego konta w serwisie Facebook przy użyciu przykładowy projekt programu ASP.NET Core 2.0, utworzony na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="02279-105">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="02279-106">Wymaga uwierzytelniania serwisu Facebook [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="02279-106">Facebook authentication requires the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package.</span></span> <span data-ttu-id="02279-107">Zaczniemy od utworzenia identyfikator aplikacji Facebook, postępując zgodnie z [oficjalnych kroków](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="02279-107">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="02279-108">Tworzenie aplikacji w usłudze Facebook</span><span class="sxs-lookup"><span data-stu-id="02279-108">Create the app in Facebook</span></span>

* <span data-ttu-id="02279-109">Przejdź do [aplikacji usługi Facebook deweloperów](https://developers.facebook.com/apps/) strony, a następnie zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="02279-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="02279-110">Jeśli nie masz jeszcze konta w serwisie Facebook, użyj **zarejestrować się za pomocą konta Facebook** łącze na stronie logowania, aby go utworzyć.</span><span class="sxs-lookup"><span data-stu-id="02279-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="02279-111">Naciśnij pozycję **Dodaj nową aplikację** przycisk w prawym górnym rogu, aby utworzyć nowy identyfikator aplikacji.</span><span class="sxs-lookup"><span data-stu-id="02279-111">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Facebook dla portalu deweloperów Otwórz w programie Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="02279-113">Wypełnij formularz, a następnie naciśnij przycisk **utworzyć identyfikator aplikacji** przycisku.</span><span class="sxs-lookup"><span data-stu-id="02279-113">Fill out the form and tap the **Create App ID** button.</span></span>

  ![Tworzenie formularza nowego Identyfikatora aplikacji](index/_static/FBNewAppId.png)

* <span data-ttu-id="02279-115">Na **wybierz produkt** kliknij **Set Up** na **logowania do usługi Facebook** karty.</span><span class="sxs-lookup"><span data-stu-id="02279-115">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

  ![Strona Ustawienia produktu](index/_static/FBProductSetup.png)

* <span data-ttu-id="02279-117">**Szybki Start** z zostanie otwarty Kreator **wybierz platformę** jako pierwszej strony.</span><span class="sxs-lookup"><span data-stu-id="02279-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="02279-118">Pomiń Kreatora teraz, klikając **ustawienia** łącza w menu po lewej stronie:</span><span class="sxs-lookup"><span data-stu-id="02279-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

  ![Pomiń Szybki Start](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="02279-120">Zostanie wyświetlona **ustawienia uwierzytelniania OAuth klienta** strony:</span><span class="sxs-lookup"><span data-stu-id="02279-120">You are presented with the **Client OAuth Settings** page:</span></span>

  ![Strona Ustawienia uwierzytelniania OAuth klienta](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="02279-122">Wprowadź identyfikator URI programistycznych dzięki */signin-facebook* dołączany do **prawidłowe OAuth identyfikatory URI przekierowań** pola (na przykład: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="02279-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="02279-123">Uwierzytelnianie serwisu Facebook skonfigurowane w dalszej części tego samouczka będzie automatycznie obsługiwać żądań w */signin-facebook* trasy do zaimplementowania przepływu uwierzytelniania OAuth.</span><span class="sxs-lookup"><span data-stu-id="02279-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="02279-124">Identyfikator URI */signin-facebook* jest ustawiony jako domyślny wywołania zwrotnego dostawcy uwierzytelniania serwisu Facebook.</span><span class="sxs-lookup"><span data-stu-id="02279-124">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="02279-125">Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowanie pośredniczące uwierzytelniania serwisu Facebook za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) Klasa.</span><span class="sxs-lookup"><span data-stu-id="02279-125">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="02279-126">Kliknij przycisk **Zapisz zmiany**.</span><span class="sxs-lookup"><span data-stu-id="02279-126">Click **Save Changes**.</span></span>

* <span data-ttu-id="02279-127">Kliknij przycisk **ustawienia** > **podstawowe** link w obszarze nawigacji po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="02279-127">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="02279-128">Na tej stronie, zwróć uwagę na Twoje `App ID` i `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="02279-128">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="02279-129">Doda zarówno do aplikacji platformy ASP.NET Core w następnej sekcji:</span><span class="sxs-lookup"><span data-stu-id="02279-129">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="02279-130">W przypadku wdrażania w witrynie konieczne ponownie przeanalizowanie **logowania do usługi Facebook** strona instalacji i zarejestrować nowy identyfikator URI publicznego.</span><span class="sxs-lookup"><span data-stu-id="02279-130">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="02279-131">Identyfikator aplikacji Facebook Store i klucza tajnego aplikacji</span><span class="sxs-lookup"><span data-stu-id="02279-131">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="02279-132">Link ustawień poufnych, takich jak Facebook `App ID` i `App Secret` z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="02279-132">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="02279-133">Do celów tego samouczka, nazwa tokeny `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="02279-133">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="02279-134">Wykonaj następujące polecenia, aby bezpiecznie przechowywać `App ID` i `App Secret` za pomocą Menedżera klucz tajny:</span><span class="sxs-lookup"><span data-stu-id="02279-134">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="02279-135">Konfigurowanie uwierzytelniania serwisu Facebook</span><span class="sxs-lookup"><span data-stu-id="02279-135">Configure Facebook Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="02279-136">Dodawanie usługi Facebook w `ConfigureServices` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="02279-136">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

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

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="02279-137">Zainstaluj [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pakietu.</span><span class="sxs-lookup"><span data-stu-id="02279-137">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="02279-138">Aby zainstalować ten pakiet za pomocą programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz pozycję **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="02279-138">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="02279-139">Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="02279-139">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="02279-140">Dodaj oprogramowaniu pośredniczącym usługi Facebook w `Configure` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="02279-140">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

<span data-ttu-id="02279-141">Zobacz [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie serwisu Facebook.</span><span class="sxs-lookup"><span data-stu-id="02279-141">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="02279-142">Opcje konfiguracji może służyć do:</span><span class="sxs-lookup"><span data-stu-id="02279-142">Configuration options can be used to:</span></span>

* <span data-ttu-id="02279-143">Żądanie różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="02279-143">Request different information about the user.</span></span>
* <span data-ttu-id="02279-144">Dodaj argumenty ciągu zapytania dostosowywaniu środowiska logowania.</span><span class="sxs-lookup"><span data-stu-id="02279-144">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="02279-145">Zaloguj się za pomocą usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="02279-145">Sign in with Facebook</span></span>

<span data-ttu-id="02279-146">Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="02279-146">Run your application and click **Log in**.</span></span> <span data-ttu-id="02279-147">Zostanie wyświetlona opcja logowania się za pomocą usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="02279-147">You see an option to sign in with Facebook.</span></span>

![Aplikacja sieci Web: użytkownik nie jest uwierzytelniony](index/_static/DoneFacebook.png)

<span data-ttu-id="02279-149">Po kliknięciu **Facebook**, nastąpi przekierowanie do usługi Facebook do uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="02279-149">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Strona uwierzytelniania serwisu Facebook](index/_static/FBLogin.png)

<span data-ttu-id="02279-151">Uwierzytelnianie serwisu Facebook żądań publicznego profilu i adres e-mail, domyślnie:</span><span class="sxs-lookup"><span data-stu-id="02279-151">Facebook authentication requests public profile and email address by default:</span></span>

![Ekranie wyrażania zgody strony uwierzytelniania usługi Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="02279-153">Po wprowadzeniu poświadczeń serwisu Facebook nastąpi przekierowanie do witryny, w którym można ustawić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="02279-153">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="02279-154">Obecnie zalogowano Cię przy użyciu poświadczeń serwisu Facebook:</span><span class="sxs-lookup"><span data-stu-id="02279-154">You are now logged in using your Facebook credentials:</span></span>

![Aplikacja sieci Web: użytkownik uwierzytelniony](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="02279-156">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="02279-156">Troubleshooting</span></span>

* <span data-ttu-id="02279-157">**Platforma ASP.NET Core 2.x tylko:** Jeśli tożsamość nie jest skonfigurowana, wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: należy podać opcję "SignInScheme"*.</span><span class="sxs-lookup"><span data-stu-id="02279-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="02279-158">Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.</span><span class="sxs-lookup"><span data-stu-id="02279-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="02279-159">Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, możesz uzyskać *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu.</span><span class="sxs-lookup"><span data-stu-id="02279-159">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="02279-160">Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.</span><span class="sxs-lookup"><span data-stu-id="02279-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="02279-161">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="02279-161">Next steps</span></span>

* <span data-ttu-id="02279-162">W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="02279-162">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="02279-163">Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="02279-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="02279-164">Gdy publikujesz witrynę sieci web do aplikacji sieci web platformy Azure, należy zresetować `AppSecret` w portalu dla deweloperów usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="02279-164">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="02279-165">Ustaw `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret` jako ustawienia aplikacji w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="02279-165">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="02279-166">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="02279-166">The configuration system is set up to read keys from environment variables.</span></span>
