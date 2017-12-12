---
title: "Ustawienia logowania zewnętrznego w usłudze Twitter"
author: rick-anderson
description: "W tym samouczku przedstawiono integracji usługi Twitter konta użytkownika uwierzytelniania do istniejącej aplikacji platformy ASP.NET Core."
keywords: Platformy ASP.NET Core, Twitter, logowania, uwierzytelniania
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: E5931607-31C0-4B20-B416-85E3550F5EA8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/twitter-logins
ms.openlocfilehash: 6751b34b42007cffa9ee92ee49170564b8eac997
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="configuring-twitter-authentication"></a><span data-ttu-id="498d6-104">Konfigurowanie uwierzytelniania usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="498d6-104">Configuring Twitter authentication</span></span>

<span data-ttu-id="498d6-105">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="498d6-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="498d6-106">W tym samouczku przedstawiono sposób użytkownicy mogli [Zaloguj się przy użyciu swojego konta w serwisie Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) przy użyciu przykładowego projektu programu ASP.NET 2.0 Core utworzony na [poprzedniej strony](index.md).</span><span class="sxs-lookup"><span data-stu-id="498d6-106">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="498d6-107">Tworzenie aplikacji w usłudze Twitter</span><span class="sxs-lookup"><span data-stu-id="498d6-107">Create the app in Twitter</span></span>

* <span data-ttu-id="498d6-108">Przejdź do [https://apps.twitter.com/](https://apps.twitter.com/) i zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="498d6-108">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="498d6-109">Jeśli nie masz jeszcze konta w usłudze Twitter, użyj  **[Zamów teraz](https://twitter.com/signup)**  łącze, aby go utworzyć.</span><span class="sxs-lookup"><span data-stu-id="498d6-109">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="498d6-110">Po zalogowaniu, **Zarządzanie aplikacjami** wyświetlana jest strona:</span><span class="sxs-lookup"><span data-stu-id="498d6-110">After signing in, the **Application Management** page is shown:</span></span>

![Zarządzanie aplikacjami Twitter Otwórz w programie Microsoft Edge](index/_static/TwitterAppManage.png)

* <span data-ttu-id="498d6-112">Wybierz **Utwórz nową aplikację** i wypełnianie aplikacji **nazwa**, **opis** i publiczne **witryny sieci Web** (może to być tymczasowy aż do identyfikatora URI Zarejestruj nazwę domeny):</span><span class="sxs-lookup"><span data-stu-id="498d6-112">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

![Tworzenie strony aplikacji](index/_static/TwitterCreate.png)

* <span data-ttu-id="498d6-114">Wprowadź identyfikator URI programowania z */signin-twitter* dołączany do **prawidłowy OAuth identyfikator URI przekierowania** pola (na przykład: `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="498d6-114">Enter your development URI with */signin-twitter* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="498d6-115">Schemat uwierzytelniania Twitter skonfigurowane w dalszej części tego samouczka automatycznie będzie obsługiwać żądań */signin-twitter* trasy do zaimplementowania przepływu OAuth.</span><span class="sxs-lookup"><span data-stu-id="498d6-115">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at */signin-twitter* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="498d6-116">Wypełnij pozostałej części formularza i wybierz **tworzenie aplikacji Twitter**.</span><span class="sxs-lookup"><span data-stu-id="498d6-116">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="498d6-117">Wyświetlane są szczegóły nowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="498d6-117">New application details are displayed:</span></span>

![Karta Szczegóły na stronie aplikacji](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="498d6-119">W przypadku wdrażania lokacji należy ponownie **Zarządzanie aplikacjami** strony i Zarejestruj nowy identyfikator URI publicznego.</span><span class="sxs-lookup"><span data-stu-id="498d6-119">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="498d6-120">Przechowywanie Twitter ConsumerKey i ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="498d6-120">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="498d6-121">Link ustawień poufnych, takich jak Twitter `Consumer Key` i `Consumer Secret` do swojej aplikacji konfiguracji za pomocą [Manager klucz tajny](../../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="498d6-121">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](../../app-secrets.md).</span></span> <span data-ttu-id="498d6-122">Do celów tego samouczka, nazwa tokeny `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="498d6-122">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="498d6-123">Tokeny te można znaleźć w **kluczy i tokenów dostępu** kartę po utworzeniu nowej aplikacji Twitter:</span><span class="sxs-lookup"><span data-stu-id="498d6-123">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Karta kluczy i tokeny dostępu](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="498d6-125">Konfigurowanie uwierzytelniania usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="498d6-125">Configure Twitter Authentication</span></span>

<span data-ttu-id="498d6-126">Szablon projektu używany w tym samouczku upewnia się, że [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) pakiet jest już zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="498d6-126">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="498d6-127">Aby zainstalować ten pakiet przy użyciu programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="498d6-127">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="498d6-128">Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="498d6-128">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="498d6-129">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="498d6-129">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="498d6-130">Dodaj usługę Twitter w `ConfigureServices` metody w *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="498d6-130">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

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

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="498d6-131">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="498d6-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="498d6-132">Dodaj oprogramowaniu pośredniczącym usługi Twitter w `Configure` metody w *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="498d6-132">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

---

<span data-ttu-id="498d6-133">Zobacz [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelniania usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="498d6-133">See the [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="498d6-134">To może być używane do żądania różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="498d6-134">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="498d6-135">Zaloguj się przy użyciu usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="498d6-135">Sign in with Twitter</span></span>

<span data-ttu-id="498d6-136">Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="498d6-136">Run your application and click **Log in**.</span></span> <span data-ttu-id="498d6-137">Pojawia się opcję, aby zalogować się przy użyciu usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="498d6-137">An option to sign in with Twitter appears:</span></span>

![Aplikacja sieci Web: użytkownik nie jest uwierzytelniony](index/_static/DoneTwitter.png)

<span data-ttu-id="498d6-139">Kliknięcie **Twitter** przekierowuje do usługi Twitter dla uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="498d6-139">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Strona uwierzytelniania w usłudze Twitter](index/_static/TwitterLogin.png)

<span data-ttu-id="498d6-141">Po wprowadzeniu poświadczeń usługi Twitter, nastąpi przekierowanie do witryny sieci web, w którym można ustawić adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="498d6-141">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="498d6-142">Użytkownik jest obecnie zalogowany przy użyciu poświadczeń konta usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="498d6-142">You are now logged in using your Twitter credentials:</span></span>

![Aplikacja sieci Web: użytkownik uwierzytelniony](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="498d6-144">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="498d6-144">Troubleshooting</span></span>

* <span data-ttu-id="498d6-145">**Platformy ASP.NET Core tylko 2.x:** Jeśli tożsamość nie jest skonfigurowana przez wywołanie metody `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: należy podać opcję "SignInScheme"*.</span><span class="sxs-lookup"><span data-stu-id="498d6-145">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="498d6-146">Szablon projektu używany w tym samouczku zapewnia to zrobić.</span><span class="sxs-lookup"><span data-stu-id="498d6-146">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="498d6-147">Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, otrzyma *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu.</span><span class="sxs-lookup"><span data-stu-id="498d6-147">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="498d6-148">Wybierz **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować mimo błędu.</span><span class="sxs-lookup"><span data-stu-id="498d6-148">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="498d6-149">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="498d6-149">Next steps</span></span>

* <span data-ttu-id="498d6-150">W tym artykule pokazano, jak można uwierzytelniać z usługą Twitter.</span><span class="sxs-lookup"><span data-stu-id="498d6-150">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="498d6-151">Można wykonać podobne podejścia do uwierzytelniania za pomocą innych dostawców wymienione na [poprzedniej strony](index.md).</span><span class="sxs-lookup"><span data-stu-id="498d6-151">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="498d6-152">Po opublikowaniu witryny sieci web do aplikacji sieci web platformy Azure, należy zresetować `ConsumerSecret` w portalu dla deweloperów usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="498d6-152">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="498d6-153">Ustaw `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret` zgodnie z ustawieniami aplikacji w portalu Azure.</span><span class="sxs-lookup"><span data-stu-id="498d6-153">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="498d6-154">System konfiguracji jest skonfigurowany do odczytu klucze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="498d6-154">The configuration system is set up to read keys from environment variables.</span></span>
