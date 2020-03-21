---
title: Ustawienia zewnętrznej nazwy logowania usługi Facebook w programie ASP.NET Core
author: rick-anderson
description: Samouczek z przykładami kodu demonstrujących integrację uwierzytelniania użytkownika konta w serwisie Facebook w istniejącej aplikacji ASP.NET Core.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: bb26a27f026e744c7d4925aa2281bf0625fff8a2
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989779"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="566a8-103">Ustawienia zewnętrznej nazwy logowania usługi Facebook w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="566a8-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="566a8-104">Autorzy [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="566a8-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="566a8-105">Ten samouczek z przykładami kodu pokazuje, jak umożliwić użytkownikom zalogowanie się za pomocą konta w serwisie Facebook przy użyciu przykładowego projektu ASP.NET Core 3,0 utworzonego na [poprzedniej stronie](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="566a8-105">This tutorial with code examples shows how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="566a8-106">Zacznij od utworzenia identyfikatora aplikacji Facebook, wykonując [czynności urzędowe](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="566a8-106">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="566a8-107">Tworzenie aplikacji w usłudze Facebook</span><span class="sxs-lookup"><span data-stu-id="566a8-107">Create the app in Facebook</span></span>

* <span data-ttu-id="566a8-108">Dodaj pakiet NuGet [Microsoft. AspNetCore. Authentication. Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) do projektu.</span><span class="sxs-lookup"><span data-stu-id="566a8-108">Add the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package to the project.</span></span>

* <span data-ttu-id="566a8-109">Przejdź do strony [aplikacji deweloperzy serwisu Facebook](https://developers.facebook.com/apps/) i zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="566a8-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="566a8-110">Jeśli nie masz jeszcze konta w serwisie Facebook, utwórz je za pomocą linku rejestracji w usłudze **Facebook** na stronie logowania.</span><span class="sxs-lookup"><span data-stu-id="566a8-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>  <span data-ttu-id="566a8-111">Gdy masz konto w serwisie Facebook, postępuj zgodnie z instrukcjami, aby zarejestrować się jako deweloper w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="566a8-111">Once you have a Facebook account, follow the instructions to register as a Facebook Developer.</span></span>

* <span data-ttu-id="566a8-112">Z menu **Moje aplikacje** wybierz pozycję **Utwórz aplikację** , aby utworzyć nowy identyfikator aplikacji.</span><span class="sxs-lookup"><span data-stu-id="566a8-112">From the **My Apps** menu select **Create App** to create a new App ID.</span></span>

   ![Facebook dla portalu deweloperów Otwórz w programie Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="566a8-114">Wypełnij formularz i naciśnij przycisk **Utwórz identyfikator aplikacji** .</span><span class="sxs-lookup"><span data-stu-id="566a8-114">Fill out the form and tap the **Create App ID** button.</span></span>

  ![Tworzenie formularza nowego Identyfikatora aplikacji](index/_static/FBNewAppId.png)

* <span data-ttu-id="566a8-116">Na nowej karcie aplikacji wybierz pozycję **Dodaj produkt**.</span><span class="sxs-lookup"><span data-stu-id="566a8-116">On the new App card, select **Add a Product**.</span></span>  <span data-ttu-id="566a8-117">Na karcie **Logowanie w serwisie Facebook** kliknij pozycję **Konfiguruj** .</span><span class="sxs-lookup"><span data-stu-id="566a8-117">On the **Facebook Login** card, click **Set Up**</span></span> 

  ![Strona Ustawienia produktu](index/_static/FBProductSetup.png)

* <span data-ttu-id="566a8-119">Zostanie uruchomiony Kreator **szybkiego startu** z **wybieraniem platformy** jako pierwszej strony.</span><span class="sxs-lookup"><span data-stu-id="566a8-119">The **Quickstart** wizard launches with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="566a8-120">Pomiń kreatora teraz, klikając link **Ustawienia** **logowania w serwisie Facebook** w menu po lewej stronie:</span><span class="sxs-lookup"><span data-stu-id="566a8-120">Bypass the wizard for now by clicking the **FaceBook Login** **Settings** link in the menu on the lower left:</span></span>

  ![Pomiń Szybki Start](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="566a8-122">Zostanie wyświetlona strona **ustawień uwierzytelniania OAuth klienta** :</span><span class="sxs-lookup"><span data-stu-id="566a8-122">You are presented with the **Client OAuth Settings** page:</span></span>

  ![Strona Ustawienia uwierzytelniania OAuth klienta](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="566a8-124">Wprowadź identyfikator URI programowania z */SignIn-facebooką* dołączoną do **prawidłowego pola URI przekierowania OAuth** (na przykład: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="566a8-124">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="566a8-125">Uwierzytelnianie w serwisie Facebook skonfigurowane w dalszej części tego samouczka będzie automatycznie obsługiwać żądania w marszrucie */SignIn-Facebook* w celu zaimplementowania przepływu OAuth.</span><span class="sxs-lookup"><span data-stu-id="566a8-125">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="566a8-126">Identyfikator URI */SignIn-Facebook* jest ustawiany jako domyślne wywołanie zwrotne dostawcy uwierzytelniania w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="566a8-126">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="566a8-127">Domyślny identyfikator URI wywołania zwrotnego można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania w serwisie Facebook za pośrednictwem dziedziczonej właściwości [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) klasy [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) .</span><span class="sxs-lookup"><span data-stu-id="566a8-127">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="566a8-128">Kliknij przycisk **Save Changes** (Zapisz zmiany).</span><span class="sxs-lookup"><span data-stu-id="566a8-128">Click **Save Changes**.</span></span>

* <span data-ttu-id="566a8-129">Kliknij pozycję **ustawienia** > łącze **podstawowe** w lewym okienku nawigacji.</span><span class="sxs-lookup"><span data-stu-id="566a8-129">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="566a8-130">Na tej stronie Zanotuj `App ID` i `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="566a8-130">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="566a8-131">Doda zarówno do aplikacji platformy ASP.NET Core w następnej sekcji:</span><span class="sxs-lookup"><span data-stu-id="566a8-131">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="566a8-132">Podczas wdrażania witryny należy ponownie odwiedzić stronę instalatora logowania do **serwisu Facebook** i zarejestrować nowy publiczny identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="566a8-132">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-the-facebook-app-id-and-secret"></a><span data-ttu-id="566a8-133">Przechowywanie identyfikatora i wpisu tajnego aplikacji w serwisie Facebook</span><span class="sxs-lookup"><span data-stu-id="566a8-133">Store the Facebook app ID and secret</span></span>

<span data-ttu-id="566a8-134">Przechowuj ustawienia poufne, takie jak identyfikator aplikacji w serwisie Facebook i wartości tajne przy użyciu [Menedżera wpisów tajnych](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="566a8-134">Store sensitive settings such as the Facebook app ID and secret values with [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="566a8-135">W tym przykładzie wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="566a8-135">For this sample, use the following steps:</span></span>

1. <span data-ttu-id="566a8-136">Zainicjuj projekt dla magazynu wpisów tajnych zgodnie z instrukcjami w obszarze [Włączanie magazynu tajnego](xref:security/app-secrets#enable-secret-storage).</span><span class="sxs-lookup"><span data-stu-id="566a8-136">Initialize the project for secret storage per the instructions at [Enable secret storage](xref:security/app-secrets#enable-secret-storage).</span></span>
1. <span data-ttu-id="566a8-137">Zapisz poufne ustawienia w lokalnym magazynie wpisów tajnych przy użyciu kluczy tajnych `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret`:</span><span class="sxs-lookup"><span data-stu-id="566a8-137">Store the sensitive settings in the local secret store with the secret keys `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`:</span></span>

    ```dotnetcli
    dotnet user-secrets set "Authentication:Facebook:AppId" "<app-id>"
    dotnet user-secrets set "Authentication:Facebook:AppSecret" "<app-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-facebook-authentication"></a><span data-ttu-id="566a8-138">Konfigurowanie uwierzytelniania serwisu Facebook</span><span class="sxs-lookup"><span data-stu-id="566a8-138">Configure Facebook Authentication</span></span>

<span data-ttu-id="566a8-139">Dodaj usługę Facebook w `ConfigureServices` metodzie w pliku *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="566a8-139">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="566a8-140">Zobacz Dokumentacja interfejsu API [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) , aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="566a8-140">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="566a8-141">Opcje konfiguracji może służyć do:</span><span class="sxs-lookup"><span data-stu-id="566a8-141">Configuration options can be used to:</span></span>

* <span data-ttu-id="566a8-142">Żądanie różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="566a8-142">Request different information about the user.</span></span>
* <span data-ttu-id="566a8-143">Dodaj argumenty ciągu zapytania dostosowywaniu środowiska logowania.</span><span class="sxs-lookup"><span data-stu-id="566a8-143">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="566a8-144">Zaloguj się za pomocą usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="566a8-144">Sign in with Facebook</span></span>

<span data-ttu-id="566a8-145">Uruchom aplikację i kliknij pozycję **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="566a8-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="566a8-146">Zostanie wyświetlona opcja logowania się za pomocą usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="566a8-146">You see an option to sign in with Facebook.</span></span>

![Aplikacja sieci Web: użytkownik nie jest uwierzytelniony](index/_static/DoneFacebook.png)

<span data-ttu-id="566a8-148">Gdy klikniesz pozycję **Facebook**, nastąpi przekierowanie do usługi Facebook w celu uwierzytelnienia:</span><span class="sxs-lookup"><span data-stu-id="566a8-148">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Strona uwierzytelniania serwisu Facebook](index/_static/FBLogin.png)

<span data-ttu-id="566a8-150">Uwierzytelnianie serwisu Facebook żądań publicznego profilu i adres e-mail, domyślnie:</span><span class="sxs-lookup"><span data-stu-id="566a8-150">Facebook authentication requests public profile and email address by default:</span></span>

![Ekranie wyrażania zgody strony uwierzytelniania usługi Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="566a8-152">Po wprowadzeniu poświadczeń serwisu Facebook nastąpi przekierowanie do witryny, w którym można ustawić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="566a8-152">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="566a8-153">Obecnie zalogowano Cię przy użyciu poświadczeń serwisu Facebook:</span><span class="sxs-lookup"><span data-stu-id="566a8-153">You are now logged in using your Facebook credentials:</span></span>

![Aplikacja sieci Web: użytkownik uwierzytelniony](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="566a8-155">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="566a8-155">Troubleshooting</span></span>

* <span data-ttu-id="566a8-156">**Tylko ASP.NET Core 2. x:** Jeśli tożsamość nie jest konfigurowana przez wywołanie `services.AddIdentity` w `ConfigureServices`, próba uwierzytelnienia spowoduje powstanie *argumentu ArgumentException: należy podać opcję "SignInScheme"* .</span><span class="sxs-lookup"><span data-stu-id="566a8-156">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="566a8-157">Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.</span><span class="sxs-lookup"><span data-stu-id="566a8-157">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="566a8-158">Jeśli baza danych lokacji nie została utworzona przez zastosowanie początkowej migracji, podczas *przetwarzania błędu żądania nie można wykonać operacji bazy danych* .</span><span class="sxs-lookup"><span data-stu-id="566a8-158">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="566a8-159">Naciśnij pozycję **Zastosuj migracje** , aby utworzyć bazę danych i odświeżyć, aby kontynuować z powodu błędu.</span><span class="sxs-lookup"><span data-stu-id="566a8-159">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="566a8-160">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="566a8-160">Next steps</span></span>

* <span data-ttu-id="566a8-161">W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="566a8-161">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="566a8-162">Podobne podejście można wykonać w celu uwierzytelnienia z innymi dostawcami wymienionymi na [poprzedniej stronie](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="566a8-162">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="566a8-163">Po opublikowaniu witryny sieci Web w usłudze Azure Web App należy zresetować `AppSecret` w portalu dla deweloperów serwisu Facebook.</span><span class="sxs-lookup"><span data-stu-id="566a8-163">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="566a8-164">Ustaw `Authentication:Facebook:AppId` i `Authentication:Facebook:AppSecret` jako ustawienia aplikacji w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="566a8-164">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="566a8-165">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="566a8-165">The configuration system is set up to read keys from environment variables.</span></span>
