---
title: Logowanie zewnętrzne konfiguracji z platformy ASP.NET Core w usłudze Twitter
author: rick-anderson
description: W tym samouczku przedstawiono integracji usługi Twitter konta użytkownika uwierzytelniania do istniejącej aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 11/01/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/twitter-logins
ms.openlocfilehash: e0bf0084f8e46f3774fa070602404840aa803661
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a><span data-ttu-id="36788-103">Logowanie zewnętrzne konfiguracji z platformy ASP.NET Core w usłudze Twitter</span><span class="sxs-lookup"><span data-stu-id="36788-103">Twitter external login setup with ASP.NET Core</span></span>

<span data-ttu-id="36788-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="36788-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="36788-105">W tym samouczku przedstawiono sposób użytkownicy mogli [Zaloguj się przy użyciu swojego konta w serwisie Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) przy użyciu przykładowego projektu programu ASP.NET 2.0 Core utworzony na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="36788-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="36788-106">Tworzenie aplikacji w usłudze Twitter</span><span class="sxs-lookup"><span data-stu-id="36788-106">Create the app in Twitter</span></span>

* <span data-ttu-id="36788-107">Przejdź do [ https://apps.twitter.com/ ](https://apps.twitter.com/) i zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="36788-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="36788-108">Jeśli nie masz jeszcze konta w usłudze Twitter, użyj **[Zamów teraz](https://twitter.com/signup)** łącze, aby go utworzyć.</span><span class="sxs-lookup"><span data-stu-id="36788-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="36788-109">Po zalogowaniu, **Zarządzanie aplikacjami** wyświetlana jest strona:</span><span class="sxs-lookup"><span data-stu-id="36788-109">After signing in, the **Application Management** page is shown:</span></span>

![Zarządzanie aplikacjami Twitter Otwórz w programie Microsoft Edge](index/_static/TwitterAppManage.png)

* <span data-ttu-id="36788-111">Wybierz **Utwórz nową aplikację** i wypełnianie aplikacji **nazwa**, **opis** i publiczne **witryny sieci Web** (może to być tymczasowy aż do identyfikatora URI Zarejestruj nazwę domeny):</span><span class="sxs-lookup"><span data-stu-id="36788-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

![Tworzenie strony aplikacji](index/_static/TwitterCreate.png)

* <span data-ttu-id="36788-113">Wprowadź identyfikator URI programowania z */signin-twitter* dołączany do **prawidłowy OAuth identyfikator URI przekierowania** pola (na przykład: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="36788-113">Enter your development URI with */signin-twitter* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="36788-114">Schemat uwierzytelniania Twitter skonfigurowane w dalszej części tego samouczka automatycznie będzie obsługiwać żądań */signin-twitter* trasy do zaimplementowania przepływu OAuth.</span><span class="sxs-lookup"><span data-stu-id="36788-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at */signin-twitter* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="36788-115">Wypełnij pozostałej części formularza i wybierz **tworzenie aplikacji Twitter**.</span><span class="sxs-lookup"><span data-stu-id="36788-115">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="36788-116">Wyświetlane są szczegóły nowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="36788-116">New application details are displayed:</span></span>

![Karta Szczegóły na stronie aplikacji](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="36788-118">W przypadku wdrażania lokacji należy ponownie **Zarządzanie aplikacjami** strony i Zarejestruj nowy identyfikator URI publicznego.</span><span class="sxs-lookup"><span data-stu-id="36788-118">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="36788-119">Przechowywanie Twitter ConsumerKey i ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="36788-119">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="36788-120">Link ustawień poufnych, takich jak Twitter `Consumer Key` i `Consumer Secret` do swojej aplikacji konfiguracji za pomocą [Manager klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="36788-120">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="36788-121">Do celów tego samouczka, nazwa tokeny `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="36788-121">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="36788-122">Tokeny te można znaleźć w **kluczy i tokenów dostępu** kartę po utworzeniu nowej aplikacji Twitter:</span><span class="sxs-lookup"><span data-stu-id="36788-122">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Karta kluczy i tokeny dostępu](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="36788-124">Konfigurowanie uwierzytelniania usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="36788-124">Configure Twitter Authentication</span></span>

<span data-ttu-id="36788-125">Szablon projektu używany w tym samouczku upewnia się, że [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) pakiet jest już zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="36788-125">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="36788-126">Aby zainstalować ten pakiet przy użyciu programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="36788-126">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="36788-127">Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="36788-127">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="36788-128">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="36788-128">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="36788-129">Dodaj usługę Twitter w `ConfigureServices` metody w *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="36788-129">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="36788-130">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="36788-130">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="36788-131">Dodaj oprogramowaniu pośredniczącym usługi Twitter w `Configure` metody w *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="36788-131">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

* * *
<span data-ttu-id="36788-132">Zobacz [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelniania usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="36788-132">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="36788-133">To może być używane do żądania różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="36788-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="36788-134">Zaloguj się przy użyciu usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="36788-134">Sign in with Twitter</span></span>

<span data-ttu-id="36788-135">Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="36788-135">Run your application and click **Log in**.</span></span> <span data-ttu-id="36788-136">Pojawia się opcję, aby zalogować się przy użyciu usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="36788-136">An option to sign in with Twitter appears:</span></span>

![Aplikacja sieci Web: użytkownik nie jest uwierzytelniony](index/_static/DoneTwitter.png)

<span data-ttu-id="36788-138">Kliknięcie **Twitter** przekierowuje do usługi Twitter dla uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="36788-138">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Strona uwierzytelniania w usłudze Twitter](index/_static/TwitterLogin.png)

<span data-ttu-id="36788-140">Po wprowadzeniu poświadczeń usługi Twitter, nastąpi przekierowanie do witryny sieci web, w którym można ustawić adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="36788-140">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="36788-141">Użytkownik jest obecnie zalogowany przy użyciu poświadczeń konta usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="36788-141">You are now logged in using your Twitter credentials:</span></span>

![Aplikacja sieci Web: użytkownik uwierzytelniony](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="36788-143">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="36788-143">Troubleshooting</span></span>

* <span data-ttu-id="36788-144">**Platformy ASP.NET Core tylko 2.x:** Jeśli tożsamości nie jest skonfigurowana pod wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: należy podać opcję "SignInScheme"*.</span><span class="sxs-lookup"><span data-stu-id="36788-144">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="36788-145">Szablon projektu używany w tym samouczku zapewnia to zrobić.</span><span class="sxs-lookup"><span data-stu-id="36788-145">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="36788-146">Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, otrzyma *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu.</span><span class="sxs-lookup"><span data-stu-id="36788-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="36788-147">Wybierz **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować mimo błędu.</span><span class="sxs-lookup"><span data-stu-id="36788-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="36788-148">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="36788-148">Next steps</span></span>

* <span data-ttu-id="36788-149">W tym artykule pokazano, jak można uwierzytelniać z usługą Twitter.</span><span class="sxs-lookup"><span data-stu-id="36788-149">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="36788-150">Można wykonać podobne podejścia do uwierzytelniania za pomocą innych dostawców wymienione na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="36788-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="36788-151">Po opublikowaniu witryny sieci web do aplikacji sieci web platformy Azure, należy zresetować `ConsumerSecret` w portalu dla deweloperów usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="36788-151">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="36788-152">Ustaw `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret` zgodnie z ustawieniami aplikacji w portalu Azure.</span><span class="sxs-lookup"><span data-stu-id="36788-152">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="36788-153">System konfiguracji jest skonfigurowany do odczytu klucze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="36788-153">The configuration system is set up to read keys from environment variables.</span></span>
