---
title: Konfiguracja logowania zewnętrznego usługi Twitter za pomocą ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono integrację uwierzytelniania użytkownika konta usługi Twitter z istniejącą aplikacją ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 5182f1647acb664bf35f086fcddbe909559a62f7
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082305"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="f4abd-103">Konfiguracja logowania zewnętrznego usługi Twitter za pomocą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f4abd-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="f4abd-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f4abd-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f4abd-105">Ten przykład pokazuje, jak umożliwić użytkownikom [zalogowanie](https://dev.twitter.com/web/sign-in/desktop-browser) się przy użyciu konta usługi Twitter za pomocą przykładowego projektu ASP.NET Core 2,2 utworzonego na [poprzedniej stronie](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="f4abd-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="f4abd-106">Tworzenie aplikacji w usłudze Twitter</span><span class="sxs-lookup"><span data-stu-id="f4abd-106">Create the app in Twitter</span></span>

* <span data-ttu-id="f4abd-107">Przejdź do [ https://apps.twitter.com/ ](https://apps.twitter.com/) i zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="f4abd-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="f4abd-108">Jeśli nie masz jeszcze konta usługi Twitter, Użyj linku Utwórz **[teraz](https://twitter.com/signup)** , aby go utworzyć.</span><span class="sxs-lookup"><span data-stu-id="f4abd-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="f4abd-109">Naciśnij pozycję **Utwórz nową aplikację** i wypełnij pola **Nazwa**aplikacji, **Opis** i publiczny identyfikator URI **witryny sieci Web** (może to być czas tymczasowy do momentu zarejestrowania nazwy domeny):</span><span class="sxs-lookup"><span data-stu-id="f4abd-109">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="f4abd-110">Wprowadź identyfikator URI programowania z `/signin-twitter` dołączonym do **prawidłowego pola URI przekierowania OAuth** (na przykład: `https://webapp128.azurewebsites.net/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="f4abd-110">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="f4abd-111">Schemat uwierzytelniania usługi Twitter skonfigurowany w dalszej części tego przykładu będzie automatycznie `/signin-twitter` obsługiwał żądania w marszrucie w celu zaimplementowania przepływu OAuth.</span><span class="sxs-lookup"><span data-stu-id="f4abd-111">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="f4abd-112">Segment `/signin-twitter` identyfikatora URI jest ustawiany jako domyślne wywołanie zwrotne dostawcy uwierzytelniania w usłudze Twitter.</span><span class="sxs-lookup"><span data-stu-id="f4abd-112">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="f4abd-113">Domyślny identyfikator URI wywołania zwrotnego można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania usługi Twitter za pomocą dziedziczonej właściwości [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) klasy [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .</span><span class="sxs-lookup"><span data-stu-id="f4abd-113">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="f4abd-114">Wypełnij resztę formularza i naciśnij pozycję **Utwórz aplikację w usłudze Twitter**.</span><span class="sxs-lookup"><span data-stu-id="f4abd-114">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="f4abd-115">Wyświetlane są nowe szczegóły aplikacji:</span><span class="sxs-lookup"><span data-stu-id="f4abd-115">New application details are displayed:</span></span>

## <a name="storing-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="f4abd-116">Przechowywanie klucza i wpisu tajnego interfejsu API użytkownika usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="f4abd-116">Storing Twitter Consumer API key and secret</span></span>

<span data-ttu-id="f4abd-117">Uruchom następujące polecenia, aby bezpiecznie przechowywać `ClientId` i `ClientSecret` używać [Menedżera wpisów tajnych](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="f4abd-117">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

<span data-ttu-id="f4abd-118">Połącz poufne ustawienia, takie `Consumer Key` jak `Consumer Secret` Twitter i konfiguracja aplikacji, za pomocą [Menedżera wpisów tajnych](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="f4abd-118">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="f4abd-119">Na potrzeby tego przykładu Nazwij tokeny `Authentication:Twitter:ConsumerKey` i. `Authentication:Twitter:ConsumerSecret`</span><span class="sxs-lookup"><span data-stu-id="f4abd-119">For the purposes of this sample, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="f4abd-120">Te tokeny można znaleźć na karcie **klucze i tokeny dostępu** po utworzeniu nowej aplikacji usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="f4abd-120">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="f4abd-121">Konfigurowanie uwierzytelniania w usłudze Twitter</span><span class="sxs-lookup"><span data-stu-id="f4abd-121">Configure Twitter Authentication</span></span>

<span data-ttu-id="f4abd-122">Dodaj usługę `ConfigureServices` Twitter do metody w pliku *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="f4abd-122">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="f4abd-123">Zobacz Dokumentacja interfejsu API [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) , aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie w usłudze Twitter.</span><span class="sxs-lookup"><span data-stu-id="f4abd-123">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="f4abd-124">Może to służyć do żądania różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="f4abd-124">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="f4abd-125">Zaloguj się przy użyciu usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="f4abd-125">Sign in with Twitter</span></span>

<span data-ttu-id="f4abd-126">Uruchom aplikację i wybierz pozycję **Zaloguj się**.</span><span class="sxs-lookup"><span data-stu-id="f4abd-126">Run the app and select **Log in**.</span></span> <span data-ttu-id="f4abd-127">Zostanie wyświetlona opcja zalogowania się przy użyciu usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="f4abd-127">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="f4abd-128">Klikanie przekierowania w usłudze **Twitter** do usługi Twitter w celu uwierzytelnienia:</span><span class="sxs-lookup"><span data-stu-id="f4abd-128">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="f4abd-129">Po wprowadzeniu poświadczeń usługi Twitter nastąpi przekierowanie do witryny sieci Web, w której można ustawić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="f4abd-129">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="f4abd-130">Użytkownik jest obecnie zalogowany przy użyciu poświadczeń usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="f4abd-130">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="f4abd-131">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="f4abd-131">Troubleshooting</span></span>

* <span data-ttu-id="f4abd-132">**Tylko ASP.NET Core 2. x:** Jeśli tożsamość nie jest skonfigurowana przez `services.AddIdentity` wywołanie `ConfigureServices`w, próba uwierzytelnienia spowoduje powstanie *argumentuexception: Należy podać*opcję "SignInScheme".</span><span class="sxs-lookup"><span data-stu-id="f4abd-132">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="f4abd-133">Szablon projektu używany w tym przykładzie zapewnia, że jest to gotowe.</span><span class="sxs-lookup"><span data-stu-id="f4abd-133">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="f4abd-134">Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, zostanie wyświetlony *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu.</span><span class="sxs-lookup"><span data-stu-id="f4abd-134">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="f4abd-135">Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.</span><span class="sxs-lookup"><span data-stu-id="f4abd-135">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f4abd-136">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="f4abd-136">Next steps</span></span>

* <span data-ttu-id="f4abd-137">W tym artykule pokazano, jak można uwierzytelnić się w usłudze Twitter.</span><span class="sxs-lookup"><span data-stu-id="f4abd-137">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="f4abd-138">Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="f4abd-138">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="f4abd-139">Po opublikowaniu witryny sieci Web w usłudze Azure Web App należy zresetować ją `ConsumerSecret` w portalu dla deweloperów w usłudze Twitter.</span><span class="sxs-lookup"><span data-stu-id="f4abd-139">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="f4abd-140">Ustaw `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret` jako ustawienia aplikacji w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="f4abd-140">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="f4abd-141">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="f4abd-141">The configuration system is set up to read keys from environment variables.</span></span>
