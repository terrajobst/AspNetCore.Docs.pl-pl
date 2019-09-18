---
title: Ustawienia logowania zewnętrznego Google w programie ASP.NET Core
author: rick-anderson
description: Ten samouczek przedstawia integracja uwierzytelniania użytkownika konta Google do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/19/2019
uid: security/authentication/google-logins
ms.openlocfilehash: e12d831d2e0a5c9acae5ea41fb4187ad4ca6b0ea
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082486"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="38fab-103">Ustawienia logowania zewnętrznego Google w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="38fab-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="38fab-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="38fab-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="38fab-105">[Starsze interfejsy API usługi Google + zostały wyłączone z 7 marca 2019](https://developers.google.com/+/api-shutdown).</span><span class="sxs-lookup"><span data-stu-id="38fab-105">[Legacy Google+ APIs have been shut down as of March 7, 2019](https://developers.google.com/+/api-shutdown).</span></span> <span data-ttu-id="38fab-106">Google + Zaloguj i deweloperzy muszą przejść do nowego systemu Google logowania.</span><span class="sxs-lookup"><span data-stu-id="38fab-106">Google+ sign in and developers must move to a new Google sign in system.</span></span> <span data-ttu-id="38fab-107">Pakiety ASP.NET Core 2,1 i 2,2 dla usługi Google Authentication zostały zaktualizowane w celu uwzględnienia zmian.</span><span class="sxs-lookup"><span data-stu-id="38fab-107">The ASP.NET Core 2.1 and 2.2 packages for Google Authentication have be updated to accommodate the changes.</span></span> <span data-ttu-id="38fab-108">Aby uzyskać więcej informacji i tymczasowe środki zaradcze dla ASP.NET Core, zobacz [ten problem](https://github.com/aspnet/AspNetCore/issues/6486)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="38fab-108">For more information and temporary mitigations for ASP.NET Core, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/6486).</span></span> <span data-ttu-id="38fab-109">Ten samouczek został zaktualizowany przy użyciu nowego procesu instalacji.</span><span class="sxs-lookup"><span data-stu-id="38fab-109">This tutorial has been updated with the new setup process.</span></span>

<span data-ttu-id="38fab-110">W tym samouczku pokazano, jak umożliwić użytkownikom logowanie się za pomocą konta Google przy użyciu projektu ASP.NET Core 2,2 utworzonego na [poprzedniej stronie](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="38fab-110">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="38fab-111">Tworzenie projektu konsoli interfejsu API firmy Google i identyfikatora klienta</span><span class="sxs-lookup"><span data-stu-id="38fab-111">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="38fab-112">Przejdź do [strony integracja z logowaniem Google w aplikacji sieci Web](https://developers.google.com/identity/sign-in/web/devconsole-project) i wybierz pozycję **Konfiguruj projekt**.</span><span class="sxs-lookup"><span data-stu-id="38fab-112">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="38fab-113">W oknie dialogowym **Konfigurowanie klienta uwierzytelniania OAuth** wybierz opcję **serwer sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="38fab-113">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="38fab-114">W polu tekstowym **autoryzowane adresy URI przekierowania** ustaw identyfikator URI przekierowania.</span><span class="sxs-lookup"><span data-stu-id="38fab-114">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="38fab-115">Na przykład:`https://localhost:5001/signin-google`</span><span class="sxs-lookup"><span data-stu-id="38fab-115">For example, `https://localhost:5001/signin-google`</span></span>
* <span data-ttu-id="38fab-116">Zapisz **Identyfikator klienta** i **klucz tajny klienta**.</span><span class="sxs-lookup"><span data-stu-id="38fab-116">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="38fab-117">Podczas wdrażania lokacji Zarejestruj nowy publiczny adres URL z poziomu **konsoli Google**.</span><span class="sxs-lookup"><span data-stu-id="38fab-117">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="38fab-118">Store Google ClientID i ClientSecret</span><span class="sxs-lookup"><span data-stu-id="38fab-118">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="38fab-119">Przechowuj ustawienia poufne, takie jak Google `Client ID` i `Client Secret` with [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="38fab-119">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="38fab-120">Na potrzeby tego samouczka Nazwij tokeny `Authentication:Google:ClientId` i: `Authentication:Google:ClientSecret`</span><span class="sxs-lookup"><span data-stu-id="38fab-120">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="38fab-121">Poświadczenia interfejsu API i użycie można zarządzać w [konsoli interfejsu API](https://console.developers.google.com/apis/dashboard).</span><span class="sxs-lookup"><span data-stu-id="38fab-121">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="38fab-122">Skonfiguruj uwierzytelnianie Google</span><span class="sxs-lookup"><span data-stu-id="38fab-122">Configure Google authentication</span></span>

<span data-ttu-id="38fab-123">Dodaj usługę Google do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="38fab-123">Add the Google service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupGoogle.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="38fab-124">Zaloguj się przy użyciu Google</span><span class="sxs-lookup"><span data-stu-id="38fab-124">Sign in with Google</span></span>

* <span data-ttu-id="38fab-125">Uruchom aplikację i kliknij pozycję **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="38fab-125">Run the app and click **Log in**.</span></span> <span data-ttu-id="38fab-126">Zostanie wyświetlona opcja zalogowania się za pomocą usługi Google.</span><span class="sxs-lookup"><span data-stu-id="38fab-126">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="38fab-127">Kliknij przycisk **Google** , który przekierowuje do usługi Google w celu uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="38fab-127">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="38fab-128">Po wprowadzeniu poświadczeń Google nastąpi przekierowanie z powrotem do witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="38fab-128">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="38fab-129">Więcej informacji o opcjach konfiguracji obsługiwanych przez uwierzytelnianie Google można znaleźć w dokumentacji interfejsuAPI.<xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions></span><span class="sxs-lookup"><span data-stu-id="38fab-129">See the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="38fab-130">Może to służyć do żądania różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="38fab-130">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="38fab-131">Zmień domyślny identyfikator URI wywołania zwrotnego</span><span class="sxs-lookup"><span data-stu-id="38fab-131">Change the default callback URI</span></span>

<span data-ttu-id="38fab-132">Segmentem identyfikatora URI `/signin-google` jest ustawiony jako zwrotnym domyślnego dostawcę uwierzytelniania serwisu Google.</span><span class="sxs-lookup"><span data-stu-id="38fab-132">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="38fab-133">Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania Google za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) klasy.</span><span class="sxs-lookup"><span data-stu-id="38fab-133">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="38fab-134">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="38fab-134">Troubleshooting</span></span>

* <span data-ttu-id="38fab-135">Jeśli logowanie nie działa i nie pojawiają się żadne błędy, przełącz się do trybu deweloperskiego, aby ułatwić debugowanie problemu.</span><span class="sxs-lookup"><span data-stu-id="38fab-135">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="38fab-136">Jeśli tożsamość nie jest skonfigurowana przez `services.AddIdentity` wywołanie `ConfigureServices`w, próba uwierzytelnienia wyników w *argumencieexception: Należy podać*opcję "SignInScheme".</span><span class="sxs-lookup"><span data-stu-id="38fab-136">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="38fab-137">Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.</span><span class="sxs-lookup"><span data-stu-id="38fab-137">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="38fab-138">Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, możesz uzyskać *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu.</span><span class="sxs-lookup"><span data-stu-id="38fab-138">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="38fab-139">Wybierz pozycję **Zastosuj migracje** , aby utworzyć bazę danych, a następnie Odśwież stronę, aby kontynuować z powodu błędu.</span><span class="sxs-lookup"><span data-stu-id="38fab-139">Select **Apply Migrations** to create the database, and refresh the page to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="38fab-140">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="38fab-140">Next steps</span></span>

* <span data-ttu-id="38fab-141">W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Google.</span><span class="sxs-lookup"><span data-stu-id="38fab-141">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="38fab-142">Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="38fab-142">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="38fab-143">Po opublikowaniu aplikacji na platformie Azure zresetuj ją `ClientSecret` w konsoli interfejsu API firmy Google.</span><span class="sxs-lookup"><span data-stu-id="38fab-143">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="38fab-144">Ustaw `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret` jako ustawienia aplikacji w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="38fab-144">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="38fab-145">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="38fab-145">The configuration system is set up to read keys from environment variables.</span></span>
