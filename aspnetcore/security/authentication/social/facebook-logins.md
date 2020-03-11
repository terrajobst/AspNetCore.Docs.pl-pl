---
title: Ustawienia zewnętrznej nazwy logowania usługi Facebook w programie ASP.NET Core
author: rick-anderson
description: Samouczek z przykładami kodu demonstrujących integrację uwierzytelniania użytkownika konta w serwisie Facebook w istniejącej aplikacji ASP.NET Core.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 12/02/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: 2e4cc04c6e7ff8e5f5701cc7f9ede73dbc1b4685
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667469"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="8a74b-103">Ustawienia zewnętrznej nazwy logowania usługi Facebook w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a74b-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="8a74b-104">Autorzy [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8a74b-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8a74b-105">Ten samouczek z przykładami kodu pokazuje, jak umożliwić użytkownikom zalogowanie się za pomocą konta w serwisie Facebook przy użyciu przykładowego projektu ASP.NET Core 3,0 utworzonego na [poprzedniej stronie](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="8a74b-105">This tutorial with code examples shows how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="8a74b-106">Zacznij od utworzenia identyfikatora aplikacji Facebook, wykonując [czynności urzędowe](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="8a74b-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="8a74b-107">Tworzenie aplikacji w usłudze Facebook</span><span class="sxs-lookup"><span data-stu-id="8a74b-107">Create the app in Facebook</span></span>

* <span data-ttu-id="8a74b-108">Dodaj pakiet NuGet [Microsoft. AspNetCore. Authentication. Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) do projektu.</span><span class="sxs-lookup"><span data-stu-id="8a74b-108">Add the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package to the project.</span></span>

* <span data-ttu-id="8a74b-109">Przejdź do strony [aplikacji deweloperzy serwisu Facebook](https://developers.facebook.com/apps/) i zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="8a74b-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="8a74b-110">Jeśli nie masz jeszcze konta w serwisie Facebook, utwórz je za pomocą linku rejestracji w usłudze **Facebook** na stronie logowania.</span><span class="sxs-lookup"><span data-stu-id="8a74b-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>  <span data-ttu-id="8a74b-111">Gdy masz konto w serwisie Facebook, postępuj zgodnie z instrukcjami, aby zarejestrować się jako deweloper w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="8a74b-111">Once you have a Facebook account, follow the instructions to register as a Facebook Developer.</span></span>

* <span data-ttu-id="8a74b-112">Z menu **Moje aplikacje** wybierz pozycję **Utwórz aplikację** , aby utworzyć nowy identyfikator aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8a74b-112">From the **My Apps** menu select **Create App** to create a new App ID.</span></span>

   ![Facebook dla portalu deweloperów Otwórz w programie Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="8a74b-114">Wypełnij formularz i naciśnij przycisk **Utwórz identyfikator aplikacji** .</span><span class="sxs-lookup"><span data-stu-id="8a74b-114">Fill out the form and tap the **Create App ID** button.</span></span>

  ![Tworzenie formularza nowego Identyfikatora aplikacji](index/_static/FBNewAppId.png)

* <span data-ttu-id="8a74b-116">Na nowej karcie aplikacji wybierz pozycję **Dodaj produkt**.</span><span class="sxs-lookup"><span data-stu-id="8a74b-116">On the new App card, select **Add a Product**.</span></span>  <span data-ttu-id="8a74b-117">Na karcie **Logowanie w serwisie Facebook** kliknij pozycję **Konfiguruj** .</span><span class="sxs-lookup"><span data-stu-id="8a74b-117">On the **Facebook Login** card, click **Set Up**</span></span> 

  ![Strona Ustawienia produktu](index/_static/FBProductSetup.png)

* <span data-ttu-id="8a74b-119">Zostanie uruchomiony Kreator **szybkiego startu** z **wybieraniem platformy** jako pierwszej strony.</span><span class="sxs-lookup"><span data-stu-id="8a74b-119">The **Quickstart** wizard launches with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="8a74b-120">Pomiń kreatora teraz, klikając link **Ustawienia** **logowania w serwisie Facebook** w menu po lewej stronie:</span><span class="sxs-lookup"><span data-stu-id="8a74b-120">Bypass the wizard for now by clicking the **FaceBook Login** **Settings** link in the menu on the lower left:</span></span>

  ![Pomiń Szybki Start](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="8a74b-122">Zostanie wyświetlona strona **ustawień uwierzytelniania OAuth klienta** :</span><span class="sxs-lookup"><span data-stu-id="8a74b-122">You are presented with the **Client OAuth Settings** page:</span></span>

  ![Strona Ustawienia uwierzytelniania OAuth klienta](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="8a74b-124">Wprowadź identyfikator URI programowania z */SignIn-facebooką* dołączoną do **prawidłowego pola URI przekierowania OAuth** (na przykład: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="8a74b-124">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="8a74b-125">Uwierzytelnianie w serwisie Facebook skonfigurowane w dalszej części tego samouczka będzie automatycznie obsługiwać żądania w marszrucie */SignIn-Facebook* w celu zaimplementowania przepływu OAuth.</span><span class="sxs-lookup"><span data-stu-id="8a74b-125">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="8a74b-126">Identyfikator URI */SignIn-Facebook* jest ustawiany jako domyślne wywołanie zwrotne dostawcy uwierzytelniania w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="8a74b-126">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="8a74b-127">Domyślny identyfikator URI wywołania zwrotnego można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania w serwisie Facebook za pośrednictwem dziedziczonej właściwości [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) klasy [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) .</span><span class="sxs-lookup"><span data-stu-id="8a74b-127">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="8a74b-128">Kliknij przycisk **Save Changes** (Zapisz zmiany).</span><span class="sxs-lookup"><span data-stu-id="8a74b-128">Click **Save Changes**.</span></span>

* <span data-ttu-id="8a74b-129">Kliknij pozycję **ustawienia** > łącze **podstawowe** w lewym okienku nawigacji.</span><span class="sxs-lookup"><span data-stu-id="8a74b-129">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="8a74b-130">Na tej stronie Zanotuj `App ID` i `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="8a74b-130">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="8a74b-131">Doda zarówno do aplikacji platformy ASP.NET Core w następnej sekcji:</span><span class="sxs-lookup"><span data-stu-id="8a74b-131">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="8a74b-132">Podczas wdrażania witryny należy ponownie odwiedzić stronę instalatora logowania do **serwisu Facebook** i zarejestrować nowy publiczny identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="8a74b-132">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="8a74b-133">Identyfikator aplikacji Facebook Store i klucza tajnego aplikacji</span><span class="sxs-lookup"><span data-stu-id="8a74b-133">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="8a74b-134">Połącz poufne ustawienia, takie jak Facebook `App ID` i `App Secret` z konfiguracją aplikacji za pomocą [Menedżera wpisów tajnych](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="8a74b-134">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="8a74b-135">Na potrzeby tego samouczka Nazwij tokeny `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="8a74b-135">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="8a74b-136">Wykonaj następujące polecenia, aby bezpiecznie przechowywać `App ID` i `App Secret` przy użyciu Menedżera wpisów tajnych:</span><span class="sxs-lookup"><span data-stu-id="8a74b-136">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="8a74b-137">Konfigurowanie uwierzytelniania serwisu Facebook</span><span class="sxs-lookup"><span data-stu-id="8a74b-137">Configure Facebook Authentication</span></span>

<span data-ttu-id="8a74b-138">Dodaj usługę Facebook w `ConfigureServices` metodzie w pliku *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="8a74b-138">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="8a74b-139">Zobacz Dokumentacja interfejsu API [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) , aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="8a74b-139">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="8a74b-140">Opcje konfiguracji może służyć do:</span><span class="sxs-lookup"><span data-stu-id="8a74b-140">Configuration options can be used to:</span></span>

* <span data-ttu-id="8a74b-141">Żądanie różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="8a74b-141">Request different information about the user.</span></span>
* <span data-ttu-id="8a74b-142">Dodaj argumenty ciągu zapytania dostosowywaniu środowiska logowania.</span><span class="sxs-lookup"><span data-stu-id="8a74b-142">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="8a74b-143">Zaloguj się za pomocą usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="8a74b-143">Sign in with Facebook</span></span>

<span data-ttu-id="8a74b-144">Uruchom aplikację i kliknij pozycję **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="8a74b-144">Run your application and click **Log in**.</span></span> <span data-ttu-id="8a74b-145">Zostanie wyświetlona opcja logowania się za pomocą usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="8a74b-145">You see an option to sign in with Facebook.</span></span>

![Aplikacja sieci Web: użytkownik nie jest uwierzytelniony](index/_static/DoneFacebook.png)

<span data-ttu-id="8a74b-147">Gdy klikniesz pozycję **Facebook**, nastąpi przekierowanie do usługi Facebook w celu uwierzytelnienia:</span><span class="sxs-lookup"><span data-stu-id="8a74b-147">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Strona uwierzytelniania serwisu Facebook](index/_static/FBLogin.png)

<span data-ttu-id="8a74b-149">Uwierzytelnianie serwisu Facebook żądań publicznego profilu i adres e-mail, domyślnie:</span><span class="sxs-lookup"><span data-stu-id="8a74b-149">Facebook authentication requests public profile and email address by default:</span></span>

![Ekranie wyrażania zgody strony uwierzytelniania usługi Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="8a74b-151">Po wprowadzeniu poświadczeń serwisu Facebook nastąpi przekierowanie do witryny, w którym można ustawić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="8a74b-151">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="8a74b-152">Obecnie zalogowano Cię przy użyciu poświadczeń serwisu Facebook:</span><span class="sxs-lookup"><span data-stu-id="8a74b-152">You are now logged in using your Facebook credentials:</span></span>

![Aplikacja sieci Web: użytkownik uwierzytelniony](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="8a74b-154">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="8a74b-154">Troubleshooting</span></span>

* <span data-ttu-id="8a74b-155">**Tylko ASP.NET Core 2. x:** Jeśli tożsamość nie jest konfigurowana przez wywołanie `services.AddIdentity` w `ConfigureServices`, próba uwierzytelnienia spowoduje powstanie *argumentu ArgumentException: należy podać opcję "SignInScheme"* .</span><span class="sxs-lookup"><span data-stu-id="8a74b-155">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="8a74b-156">Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.</span><span class="sxs-lookup"><span data-stu-id="8a74b-156">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="8a74b-157">Jeśli baza danych lokacji nie została utworzona przez zastosowanie początkowej migracji, podczas *przetwarzania błędu żądania nie można wykonać operacji bazy danych* .</span><span class="sxs-lookup"><span data-stu-id="8a74b-157">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="8a74b-158">Naciśnij pozycję **Zastosuj migracje** , aby utworzyć bazę danych i odświeżyć, aby kontynuować z powodu błędu.</span><span class="sxs-lookup"><span data-stu-id="8a74b-158">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a74b-159">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="8a74b-159">Next steps</span></span>

* <span data-ttu-id="8a74b-160">W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="8a74b-160">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="8a74b-161">Podobne podejście można wykonać w celu uwierzytelnienia z innymi dostawcami wymienionymi na [poprzedniej stronie](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="8a74b-161">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="8a74b-162">Po opublikowaniu witryny sieci Web w usłudze Azure Web App należy zresetować `AppSecret` w portalu dla deweloperów serwisu Facebook.</span><span class="sxs-lookup"><span data-stu-id="8a74b-162">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="8a74b-163">Ustaw `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret` jako ustawienia aplikacji w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="8a74b-163">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="8a74b-164">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="8a74b-164">The configuration system is set up to read keys from environment variables.</span></span>
