---
title: Konfiguracja zewnętrznego logowania do usługi Google w ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono integrację uwierzytelniania użytkownika konta Google z istniejącą aplikacją ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/30/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 83f45143eca1be43410880bfd875a3fce1d2e9c9
ms.sourcegitcommit: de0fc77487a4d342bcc30965ec5c142d10d22c03
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2019
ms.locfileid: "73143452"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="7e368-103">Konfiguracja zewnętrznego logowania do usługi Google w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7e368-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="7e368-104">Autorzy [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7e368-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7e368-105">W tym samouczku pokazano, jak umożliwić użytkownikom logowanie się za pomocą konta Google przy użyciu projektu ASP.NET Core 3,0 utworzonego na [poprzedniej stronie](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="7e368-105">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="7e368-106">Tworzenie projektu konsoli interfejsu API firmy Google i identyfikatora klienta</span><span class="sxs-lookup"><span data-stu-id="7e368-106">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="7e368-107">Zainstaluj [Microsoft. AspNetCore. Authentication. Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google).</span><span class="sxs-lookup"><span data-stu-id="7e368-107">Install [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google).</span></span>
* <span data-ttu-id="7e368-108">Przejdź do [strony integracja z logowaniem Google w aplikacji sieci Web](https://developers.google.com/identity/sign-in/web/devconsole-project) i wybierz pozycję **Konfiguruj projekt**.</span><span class="sxs-lookup"><span data-stu-id="7e368-108">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="7e368-109">W oknie dialogowym **Konfigurowanie klienta uwierzytelniania OAuth** wybierz opcję **serwer sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="7e368-109">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="7e368-110">W polu tekstowym **autoryzowane adresy URI przekierowania** ustaw identyfikator URI przekierowania.</span><span class="sxs-lookup"><span data-stu-id="7e368-110">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="7e368-111">Na przykład:`https://localhost:44312/signin-google`</span><span class="sxs-lookup"><span data-stu-id="7e368-111">For example, `https://localhost:44312/signin-google`</span></span>
* <span data-ttu-id="7e368-112">Zapisz **Identyfikator klienta** i **klucz tajny klienta**.</span><span class="sxs-lookup"><span data-stu-id="7e368-112">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="7e368-113">Podczas wdrażania lokacji Zarejestruj nowy publiczny adres URL z poziomu **konsoli Google**.</span><span class="sxs-lookup"><span data-stu-id="7e368-113">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="7e368-114">Przechowywanie usług Google ClientID i ClientSecret</span><span class="sxs-lookup"><span data-stu-id="7e368-114">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="7e368-115">Przechowuj ustawienia poufne, takie jak Google `Client ID` i `Client Secret`, za pomocą [Menedżera wpisów tajnych](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7e368-115">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="7e368-116">Na potrzeby tego samouczka Nazwij `Authentication:Google:ClientId` tokeny i `Authentication:Google:ClientSecret`:</span><span class="sxs-lookup"><span data-stu-id="7e368-116">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "<client id>"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="7e368-117">Poświadczenia interfejsu API i użycie można zarządzać w [konsoli interfejsu API](https://console.developers.google.com/apis/dashboard).</span><span class="sxs-lookup"><span data-stu-id="7e368-117">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="7e368-118">Skonfiguruj uwierzytelnianie Google</span><span class="sxs-lookup"><span data-stu-id="7e368-118">Configure Google authentication</span></span>

<span data-ttu-id="7e368-119">Dodaj usługę Google do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7e368-119">Add the Google service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupGoogle3x.cs?highlight=11-19)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="7e368-120">Zaloguj się przy użyciu usługi Google</span><span class="sxs-lookup"><span data-stu-id="7e368-120">Sign in with Google</span></span>

* <span data-ttu-id="7e368-121">Uruchom aplikację i kliknij pozycję **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="7e368-121">Run the app and click **Log in**.</span></span> <span data-ttu-id="7e368-122">Zostanie wyświetlona opcja zalogowania się za pomocą usługi Google.</span><span class="sxs-lookup"><span data-stu-id="7e368-122">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="7e368-123">Kliknij przycisk **Google** , który przekierowuje do usługi Google w celu uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="7e368-123">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="7e368-124">Po wprowadzeniu poświadczeń Google nastąpi przekierowanie z powrotem do witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7e368-124">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="7e368-125">Więcej informacji o opcjach konfiguracji obsługiwanych przez uwierzytelnianie Google można znaleźć w dokumentacji interfejsu API <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>.</span><span class="sxs-lookup"><span data-stu-id="7e368-125">See the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="7e368-126">Może to służyć do żądania różnych informacji o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="7e368-126">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="7e368-127">Zmień domyślny identyfikator URI wywołania zwrotnego</span><span class="sxs-lookup"><span data-stu-id="7e368-127">Change the default callback URI</span></span>

<span data-ttu-id="7e368-128">Segment identyfikatora URI `/signin-google` jest ustawiany jako domyślne wywołanie zwrotne dostawcy usługi Google Authentication.</span><span class="sxs-lookup"><span data-stu-id="7e368-128">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="7e368-129">Domyślny identyfikator URI wywołania zwrotnego można zmienić podczas konfigurowania oprogramowania pośredniczącego usługi Google Authentication za pośrednictwem dziedziczonej właściwości [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) klasy [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) .</span><span class="sxs-lookup"><span data-stu-id="7e368-129">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7e368-130">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="7e368-130">Troubleshooting</span></span>

* <span data-ttu-id="7e368-131">Jeśli logowanie nie działa i nie pojawiają się żadne błędy, przełącz się do trybu deweloperskiego, aby ułatwić debugowanie problemu.</span><span class="sxs-lookup"><span data-stu-id="7e368-131">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="7e368-132">Jeśli tożsamość nie jest konfigurowana przez wywołanie `services.AddIdentity` w `ConfigureServices`, próba uwierzytelnienia wyników w *argumencieexception: należy podać opcję "SignInScheme"* .</span><span class="sxs-lookup"><span data-stu-id="7e368-132">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="7e368-133">Szablon projektu używany w tym samouczku zapewnia, że jest to gotowe.</span><span class="sxs-lookup"><span data-stu-id="7e368-133">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="7e368-134">Jeśli baza danych lokacji nie została utworzona przez zastosowanie początkowej migracji, podczas *przetwarzania błędu żądania nie można wykonać operacji bazy danych* .</span><span class="sxs-lookup"><span data-stu-id="7e368-134">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="7e368-135">Wybierz pozycję **Zastosuj migracje** , aby utworzyć bazę danych, a następnie Odśwież stronę, aby kontynuować z powodu błędu.</span><span class="sxs-lookup"><span data-stu-id="7e368-135">Select **Apply Migrations** to create the database, and refresh the page to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e368-136">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="7e368-136">Next steps</span></span>

* <span data-ttu-id="7e368-137">W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Google.</span><span class="sxs-lookup"><span data-stu-id="7e368-137">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="7e368-138">Podobne podejście można wykonać w celu uwierzytelnienia z innymi dostawcami wymienionymi na [poprzedniej stronie](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="7e368-138">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="7e368-139">Po opublikowaniu aplikacji na platformie Azure Zresetuj `ClientSecret` w konsoli interfejsu API firmy Google.</span><span class="sxs-lookup"><span data-stu-id="7e368-139">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="7e368-140">Ustaw `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret` jako ustawienia aplikacji w Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="7e368-140">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="7e368-141">System konfiguracji jest skonfigurowany do odczytywania kluczy ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="7e368-141">The configuration system is set up to read keys from environment variables.</span></span>
