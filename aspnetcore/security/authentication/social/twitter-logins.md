---
title: Konfiguracja zewnętrznych logowania za pomocą platformy ASP.NET Core w usłudze Twitter
author: rick-anderson
description: Ten samouczek przedstawia integracja uwierzytelniania użytkownika konta usługi Twitter do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 5/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 486d58b600ca5326a0728de40bb386fbb9440f67
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65516885"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="b139e-103">Konfiguracja zewnętrznych logowania za pomocą platformy ASP.NET Core w usłudze Twitter</span><span class="sxs-lookup"><span data-stu-id="b139e-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="b139e-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b139e-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b139e-105">W tym przykładzie pokazano, jak umożliwić użytkownikom [Zaloguj się przy użyciu konta w serwisie Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) przy użyciu przykładowy projekt programu ASP.NET Core 2.2 tworzone na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="b139e-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="b139e-106">Tworzenie aplikacji w usłudze Twitter</span><span class="sxs-lookup"><span data-stu-id="b139e-106">Create the app in Twitter</span></span>

* <span data-ttu-id="b139e-107">Przejdź do [ https://apps.twitter.com/ ](https://apps.twitter.com/) i zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="b139e-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="b139e-108">Jeśli nie masz jeszcze konta w serwisie Twitter, użyj **[Zarejestruj się teraz](https://twitter.com/signup)** łącze, aby go utworzyć.</span><span class="sxs-lookup"><span data-stu-id="b139e-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="b139e-109">Naciśnij pozycję **Utwórz nową aplikację** i wypełnij zgłoszenie **nazwa**, **opis** i publicznych **witryny sieci Web** identyfikatora URI (może to być tymczasowy do momentu Zarejestruj nazwy domeny):</span><span class="sxs-lookup"><span data-stu-id="b139e-109">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="b139e-110">Wprowadź identyfikator URI programistycznych dzięki `/signin-twitter` dołączany do **prawidłowe OAuth identyfikatory URI przekierowań** pola (na przykład: `https://webapp128.azurewebsites.net/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="b139e-110">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="b139e-111">Schemat uwierzytelniania usługi Twitter, skonfigurować je później w tym przykładzie będzie automatycznie obsługiwać żądań w `/signin-twitter` trasy do zaimplementowania przepływu uwierzytelniania OAuth.</span><span class="sxs-lookup"><span data-stu-id="b139e-111">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="b139e-112">Segmentem identyfikatora URI `/signin-twitter` jest ustawiony jako domyślny wywołania zwrotnego dostawcy uwierzytelniania usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="b139e-112">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="b139e-113">Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania usługi Twitter za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) Klasa.</span><span class="sxs-lookup"><span data-stu-id="b139e-113">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="b139e-114">Wypełnij pozostałej części formularza, a następnie naciśnij pozycję **tworzenie aplikacji usługi Twitter**.</span><span class="sxs-lookup"><span data-stu-id="b139e-114">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="b139e-115">Wyświetlane są szczegóły nowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="b139e-115">New application details are displayed:</span></span>

## <a name="storing-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="b139e-116">Przechowywanie klucza interfejsu API klienta usługi Twitter i wpisu tajnego</span><span class="sxs-lookup"><span data-stu-id="b139e-116">Storing Twitter Consumer API key and secret</span></span>

<span data-ttu-id="b139e-117">Uruchom następujące polecenia, aby bezpiecznie przechowywać `ClientId` i `ClientSecret` przy użyciu [Menedżera klucz tajny](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="b139e-117">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerAPISecret <Secret>
```

<span data-ttu-id="b139e-118">Link ustawień poufnych, takich jak Twitter `Consumer Key` i `Consumer Secret` z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="b139e-118">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="b139e-119">Do celów tego przykładu, nazwa tokeny `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="b139e-119">For the purposes of this sample, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="b139e-120">Tokeny te można znaleźć na **klucze i tokeny dostępu** kartę po utworzeniu nowej aplikacji usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="b139e-120">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="b139e-121">Konfigurowanie uwierzytelniania usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="b139e-121">Configure Twitter Authentication</span></span>

<span data-ttu-id="b139e-122">Dodaj usługę Twitter w `ConfigureServices` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="b139e-122">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="b139e-123">Zobacz [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianiem usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="b139e-123">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="b139e-124">Może to służyć do żądania różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="b139e-124">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="b139e-125">Zaloguj się przy użyciu usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="b139e-125">Sign in with Twitter</span></span>

<span data-ttu-id="b139e-126">Uruchom aplikację i wybierz **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="b139e-126">Run the app and select **Log in**.</span></span> <span data-ttu-id="b139e-127">Zostanie wyświetlona opcja Zaloguj się przy użyciu usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="b139e-127">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="b139e-128">Kliknięcie **Twitter** przekierowuje do usługi Twitter do uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="b139e-128">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="b139e-129">Po wprowadzeniu swoje poświadczenia usługi Twitter, nastąpi przekierowanie do witryny sieci web, w którym można ustawić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="b139e-129">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="b139e-130">Obecnie zalogowano Cię przy użyciu poświadczeń usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="b139e-130">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="b139e-131">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="b139e-131">Troubleshooting</span></span>

* <span data-ttu-id="b139e-132">**ASP.NET Core 2.x tylko:** Jeśli tożsamość nie jest skonfigurowana, wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: Opcja "SignInScheme" musi być podana*.</span><span class="sxs-lookup"><span data-stu-id="b139e-132">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="b139e-133">Szablon projektu, używane w tym przykładzie gwarantuje, że odbywa się.</span><span class="sxs-lookup"><span data-stu-id="b139e-133">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="b139e-134">Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, zostanie wyświetlony *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu.</span><span class="sxs-lookup"><span data-stu-id="b139e-134">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="b139e-135">Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.</span><span class="sxs-lookup"><span data-stu-id="b139e-135">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b139e-136">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="b139e-136">Next steps</span></span>

* <span data-ttu-id="b139e-137">W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="b139e-137">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="b139e-138">Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="b139e-138">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="b139e-139">Gdy publikujesz witrynę sieci web do aplikacji sieci web platformy Azure, należy zresetować `ConsumerSecret` w portalu dla deweloperów usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="b139e-139">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="b139e-140">Ustaw `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret` jako ustawienia aplikacji w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="b139e-140">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="b139e-141">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="b139e-141">The configuration system is set up to read keys from environment variables.</span></span>