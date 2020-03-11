---
title: Konfiguracja logowania zewnętrznego usługi Twitter za pomocą ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono integrację uwierzytelniania użytkownika konta usługi Twitter z istniejącą aplikacją ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: 4710c033018710ce3620f8d7221ae2253b2c0b69
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665929"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="336ad-103">Konfiguracja logowania zewnętrznego usługi Twitter za pomocą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="336ad-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="336ad-104">Autorzy [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="336ad-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="336ad-105">Ten przykład pokazuje, jak umożliwić użytkownikom [zalogowanie](https://dev.twitter.com/web/sign-in/desktop-browser) się przy użyciu konta usługi Twitter za pomocą przykładowego projektu ASP.NET Core 3,0 utworzonego na [poprzedniej stronie](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="336ad-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="336ad-106">Tworzenie aplikacji w usłudze Twitter</span><span class="sxs-lookup"><span data-stu-id="336ad-106">Create the app in Twitter</span></span>

* <span data-ttu-id="336ad-107">Dodaj pakiet NuGet [Microsoft. AspNetCore. Authentication. Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) do projektu.</span><span class="sxs-lookup"><span data-stu-id="336ad-107">Add the [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) NuGet package to the project.</span></span>

* <span data-ttu-id="336ad-108">Przejdź do [https://apps.twitter.com/](https://apps.twitter.com/) i zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="336ad-108">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="336ad-109">Jeśli nie masz jeszcze konta usługi Twitter, Użyj linku Utwórz **[teraz](https://twitter.com/signup)** , aby go utworzyć.</span><span class="sxs-lookup"><span data-stu-id="336ad-109">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="336ad-110">Wybierz pozycję **Utwórz aplikację**.</span><span class="sxs-lookup"><span data-stu-id="336ad-110">Select **Create an app**.</span></span> <span data-ttu-id="336ad-111">Wypełnij pola **Nazwa aplikacji**, **Opis aplikacji** i publiczny identyfikator URI **witryny sieci Web** (może to być czas tymczasowy do momentu zarejestrowania nazwy domeny):</span><span class="sxs-lookup"><span data-stu-id="336ad-111">Fill out the **App name**, **Application description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="336ad-112">Zaznacz pole wyboru obok pozycji **Włącz logowanie za pomocą usługi Twitter**</span><span class="sxs-lookup"><span data-stu-id="336ad-112">Check the box next to **Enable Sign in with Twitter**</span></span>

* <span data-ttu-id="336ad-113">Microsoft. AspNetCore. Identity wymaga, aby użytkownicy domyślnie mieli adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="336ad-113">Microsoft.AspNetCore.Identity requires users to have an email address by default.</span></span> <span data-ttu-id="336ad-114">Przejdź do karty **uprawnienia** , kliknij przycisk **Edytuj** , a następnie zaznacz pole wyboru obok **żądania adresu e-mail od użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="336ad-114">Go to the **Permissions** tab, click the **Edit** button and check the box next to **Request email address from users**.</span></span>

* <span data-ttu-id="336ad-115">Wprowadź identyfikator URI programowania z `/signin-twitter` dołączony do pola **adresy URL wywołania zwrotnego** (na przykład: `https://webapp128.azurewebsites.net/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="336ad-115">Enter your development URI with `/signin-twitter` appended into the **Callback URLs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="336ad-116">Schemat uwierzytelniania usługi Twitter skonfigurowany w dalszej części tego przykładu będzie automatycznie obsługiwał żądania na trasie `/signin-twitter` w celu zaimplementowania przepływu OAuth.</span><span class="sxs-lookup"><span data-stu-id="336ad-116">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="336ad-117">Segment identyfikatora URI `/signin-twitter` jest ustawiany jako domyślne wywołanie zwrotne dostawcy uwierzytelniania w usłudze Twitter.</span><span class="sxs-lookup"><span data-stu-id="336ad-117">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="336ad-118">Domyślny identyfikator URI wywołania zwrotnego można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania usługi Twitter za pomocą dziedziczonej właściwości [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) klasy [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .</span><span class="sxs-lookup"><span data-stu-id="336ad-118">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="336ad-119">Wypełnij resztę formularza i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="336ad-119">Fill out the rest of the form and select **Create**.</span></span> <span data-ttu-id="336ad-120">Wyświetlane są nowe szczegóły aplikacji:</span><span class="sxs-lookup"><span data-stu-id="336ad-120">New application details are displayed:</span></span>

## <a name="storing-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="336ad-121">Przechowywanie klucza i wpisu tajnego interfejsu API użytkownika usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="336ad-121">Storing Twitter Consumer API key and secret</span></span>

<span data-ttu-id="336ad-122">Uruchom następujące polecenia, aby bezpiecznie przechowywać `ClientId` i `ClientSecret` przy użyciu [Menedżera wpisów tajnych](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="336ad-122">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

<span data-ttu-id="336ad-123">Połącz poufne ustawienia, takie jak Twitter `Consumer Key` i `Consumer Secret` z konfiguracją aplikacji za pomocą [Menedżera wpisów tajnych](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="336ad-123">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="336ad-124">Na potrzeby tego przykładu Nazwij tokeny `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="336ad-124">For the purposes of this sample, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="336ad-125">Te tokeny można znaleźć na karcie **klucze i tokeny dostępu** po utworzeniu nowej aplikacji usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="336ad-125">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="336ad-126">Konfigurowanie uwierzytelniania w usłudze Twitter</span><span class="sxs-lookup"><span data-stu-id="336ad-126">Configure Twitter Authentication</span></span>

<span data-ttu-id="336ad-127">Dodaj usługę Twitter do metody `ConfigureServices` w pliku *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="336ad-127">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-15)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="336ad-128">Zobacz Dokumentacja interfejsu API [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) , aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie w usłudze Twitter.</span><span class="sxs-lookup"><span data-stu-id="336ad-128">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="336ad-129">Może to służyć do żądania różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="336ad-129">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="336ad-130">Zaloguj się przy użyciu usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="336ad-130">Sign in with Twitter</span></span>

<span data-ttu-id="336ad-131">Uruchom aplikację i wybierz pozycję **Zaloguj się**.</span><span class="sxs-lookup"><span data-stu-id="336ad-131">Run the app and select **Log in**.</span></span> <span data-ttu-id="336ad-132">Zostanie wyświetlona opcja zalogowania się przy użyciu usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="336ad-132">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="336ad-133">Klikanie przekierowania w usłudze **Twitter** do usługi Twitter w celu uwierzytelnienia:</span><span class="sxs-lookup"><span data-stu-id="336ad-133">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="336ad-134">Po wprowadzeniu poświadczeń usługi Twitter nastąpi przekierowanie do witryny sieci Web, w której można ustawić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="336ad-134">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="336ad-135">Użytkownik jest obecnie zalogowany przy użyciu poświadczeń usługi Twitter:</span><span class="sxs-lookup"><span data-stu-id="336ad-135">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="336ad-136">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="336ad-136">Troubleshooting</span></span>

* <span data-ttu-id="336ad-137">**Tylko ASP.NET Core 2. x:** Jeśli tożsamość nie jest konfigurowana przez wywołanie `services.AddIdentity` w `ConfigureServices`, próba uwierzytelnienia spowoduje powstanie *argumentu ArgumentException: należy podać opcję "SignInScheme"* .</span><span class="sxs-lookup"><span data-stu-id="336ad-137">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="336ad-138">Szablon projektu używany w tym przykładzie zapewnia, że jest to gotowe.</span><span class="sxs-lookup"><span data-stu-id="336ad-138">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="336ad-139">Jeśli baza danych lokacji nie została utworzona przez zastosowanie początkowej migracji, *podczas przetwarzania błędu żądania nie powiodła się operacja bazy danych* .</span><span class="sxs-lookup"><span data-stu-id="336ad-139">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="336ad-140">Naciśnij pozycję **Zastosuj migracje** , aby utworzyć bazę danych i odświeżyć, aby kontynuować z powodu błędu.</span><span class="sxs-lookup"><span data-stu-id="336ad-140">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="336ad-141">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="336ad-141">Next steps</span></span>

* <span data-ttu-id="336ad-142">W tym artykule pokazano, jak można uwierzytelnić się w usłudze Twitter.</span><span class="sxs-lookup"><span data-stu-id="336ad-142">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="336ad-143">Podobne podejście można wykonać w celu uwierzytelnienia z innymi dostawcami wymienionymi na [poprzedniej stronie](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="336ad-143">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="336ad-144">Po opublikowaniu witryny sieci Web w usłudze Azure Web App należy zresetować `ConsumerSecret` w portalu dla deweloperów w usłudze Twitter.</span><span class="sxs-lookup"><span data-stu-id="336ad-144">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="336ad-145">Ustaw `Authentication:Twitter:ConsumerKey` i `Authentication:Twitter:ConsumerSecret` jako ustawienia aplikacji w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="336ad-145">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="336ad-146">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="336ad-146">The configuration system is set up to read keys from environment variables.</span></span>
