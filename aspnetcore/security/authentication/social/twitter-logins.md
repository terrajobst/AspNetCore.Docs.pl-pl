---
title: Konfiguracja logowania zewnętrznego za pomocą platformy ASP.NET Core w usłudze Twitter
author: rick-anderson
description: Ten samouczek przedstawia integracja uwierzytelniania użytkownika konta usługi Twitter do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 49db8b921fde169380ca284f46e535786b2b8a30
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735807"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a><span data-ttu-id="4c95f-103">Konfiguracja logowania zewnętrznego za pomocą platformy ASP.NET Core w usłudze Twitter</span><span class="sxs-lookup"><span data-stu-id="4c95f-103">Twitter external login setup with ASP.NET Core</span></span>

<span data-ttu-id="4c95f-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4c95f-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4c95f-105">W tym samouczku pokazano, jak umożliwić użytkownikom [Zaloguj się przy użyciu konta w serwisie Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) przy użyciu przykładowy projekt programu ASP.NET Core 2.0 tworzone na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="4c95f-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="4c95f-106">Tworzenie aplikacji w usłudze Twitter</span><span class="sxs-lookup"><span data-stu-id="4c95f-106">Create the app in Twitter</span></span>

* <span data-ttu-id="4c95f-107">Przejdź do [ https://apps.twitter.com/ ](https://apps.twitter.com/) i zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="4c95f-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="4c95f-108">Jeśli nie masz jeszcze konta w serwisie Twitter, użyj **[Zarejestruj się teraz](https://twitter.com/signup)** łącze, aby go utworzyć.</span><span class="sxs-lookup"><span data-stu-id="4c95f-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="4c95f-109">Po zalogowaniu **Zarządzanie aplikacjami** wyświetlana strona:</span><span class="sxs-lookup"><span data-stu-id="4c95f-109">After signing in, the **Application Management** page is shown:</span></span>

  ![Zarządzanie aplikacjami usługi Twitter, Otwórz w programie Microsoft Edge](index/_static/TwitterAppManage.png)

* <span data-ttu-id="4c95f-111">Naciśnij pozycję **Utwórz nową aplikację** i wypełnij zgłoszenie **nazwa**, **opis** i publicznych **witryny sieci Web** identyfikatora URI (może to być tymczasowy do momentu Zarejestruj nazwy domeny):</span><span class="sxs-lookup"><span data-stu-id="4c95f-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

  ![Tworzenie strony aplikacji](index/_static/TwitterCreate.png)

* <span data-ttu-id="4c95f-113">Wprowadź identyfikator URI programistycznych dzięki `/signin-twitter` dołączany do **prawidłowe OAuth identyfikatory URI przekierowań** pola (na przykład: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="4c95f-113">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="4c95f-114">Schemat uwierzytelniania usługi Twitter, które są skonfigurowane w dalszej części tego samouczka będzie automatycznie obsługiwać żądań w `/signin-twitter` trasy do zaimplementowania przepływu uwierzytelniania OAuth.</span><span class="sxs-lookup"><span data-stu-id="4c95f-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="4c95f-115">Segmentem identyfikatora URI `/signin-twitter` jest ustawiony jako domyślny wywołania zwrotnego dostawcy uwierzytelniania usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="4c95f-115">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="4c95f-116">Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania usługi Twitter za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) Klasa.</span><span class="sxs-lookup"><span data-stu-id="4c95f-116">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="4c95f-117">Wypełnij pozostałej części formularza, a następnie naciśnij pozycję **tworzenie aplikacji usługi Twitter**.</span><span class="sxs-lookup"><span data-stu-id="4c95f-117">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="4c95f-118">Wyświetlane są szczegóły nowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4c95f-118">New application details are displayed:</span></span>

  ![Karta szczegółów na stronie aplikacji](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="4c95f-120">W przypadku wdrażania w witrynie będzie konieczne ponownie przeanalizowanie **Zarządzanie aplikacjami** strony, a następnie zarejestrować nowy identyfikator URI publicznego.</span><span class="sxs-lookup"><span data-stu-id="4c95f-120">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="4c95f-121">Przechowywanie ConsumerKey usługi Twitter i ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="4c95f-121">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="4c95f-122">Link ustawień poufnych, takich jak Twitter `Consumer Key` i `Consumer Secret` z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="4c95f-122">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="4c95f-123">Do celów tego samouczka, nazwa tokeny `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="4c95f-123">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="4c95f-124">Tokeny te można znaleźć na **klucze i tokeny dostępu** kartę po utworzeniu nowej aplikacji usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="4c95f-124">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Karta klucze i tokeny dostępu](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="4c95f-126">Konfigurowanie uwierzytelniania usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="4c95f-126">Configure Twitter Authentication</span></span>

<span data-ttu-id="4c95f-127">Szablon projektu, w tym samouczku używane zapewnia, że [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) pakiet jest już zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="4c95f-127">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="4c95f-128">Aby zainstalować ten pakiet za pomocą programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz pozycję **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4c95f-128">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="4c95f-129">Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="4c95f-129">To install with .NET Core CLI, execute the following in your project directory:</span></span>

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4c95f-130">Dodaj usługę Twitter w `ConfigureServices` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="4c95f-130">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="4c95f-131">Dodaj oprogramowaniu pośredniczącym usługi Twitter w `Configure` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="4c95f-131">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

<span data-ttu-id="4c95f-132">Zobacz [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianiem usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="4c95f-132">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="4c95f-133">Może to służyć do żądania różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="4c95f-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="4c95f-134">Zaloguj się przy użyciu usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="4c95f-134">Sign in with Twitter</span></span>

<span data-ttu-id="4c95f-135">Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="4c95f-135">Run your application and click **Log in**.</span></span> <span data-ttu-id="4c95f-136">Zostanie wyświetlona opcja Zaloguj się przy użyciu usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="4c95f-136">An option to sign in with Twitter appears:</span></span>

![Aplikacja sieci Web: Użytkownik nie jest uwierzytelniony](index/_static/DoneTwitter.png)

<span data-ttu-id="4c95f-138">Kliknięcie **Twitter** przekierowuje do usługi Twitter do uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="4c95f-138">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Strona uwierzytelniania w usłudze Twitter](index/_static/TwitterLogin.png)

<span data-ttu-id="4c95f-140">Po wprowadzeniu swoje poświadczenia usługi Twitter, nastąpi przekierowanie do witryny sieci web, w którym można ustawić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="4c95f-140">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="4c95f-141">Obecnie zalogowano Cię przy użyciu poświadczeń usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="4c95f-141">You are now logged in using your Twitter credentials:</span></span>

![Aplikacja sieci Web: Użytkownik uwierzytelniony](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="4c95f-143">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="4c95f-143">Troubleshooting</span></span>

* <span data-ttu-id="4c95f-144">**ASP.NET Core 2.x tylko:** Jeśli tożsamość nie jest skonfigurowana, wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: Opcja "SignInScheme" musi być podana*.</span><span class="sxs-lookup"><span data-stu-id="4c95f-144">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="4c95f-145">Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.</span><span class="sxs-lookup"><span data-stu-id="4c95f-145">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="4c95f-146">Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, zostanie wyświetlony *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu.</span><span class="sxs-lookup"><span data-stu-id="4c95f-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="4c95f-147">Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.</span><span class="sxs-lookup"><span data-stu-id="4c95f-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c95f-148">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="4c95f-148">Next steps</span></span>

* <span data-ttu-id="4c95f-149">W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="4c95f-149">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="4c95f-150">Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="4c95f-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="4c95f-151">Gdy publikujesz witrynę sieci web do aplikacji sieci web platformy Azure, należy zresetować `ConsumerSecret` w portalu dla deweloperów usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="4c95f-151">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="4c95f-152">Ustaw `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret` jako ustawienia aplikacji w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="4c95f-152">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="4c95f-153">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="4c95f-153">The configuration system is set up to read keys from environment variables.</span></span>
