---
title: Konfigurowanie logowania zewnętrznego Account firmy Microsoft za pomocą programu ASP.NET Core
author: rick-anderson
description: Niniejszy przykład pokazuje, integracja uwierzytelniania użytkownika konta Microsoft do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 5/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 16ec2d5f2bccc59958b884869ef42af9cfa13df0
ms.sourcegitcommit: 06a455d63ff7d6b571ca832e8117f4ac9d646baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67316592"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="e4cac-103">Konfigurowanie logowania zewnętrznego Account firmy Microsoft za pomocą programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e4cac-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="e4cac-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e4cac-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e4cac-105">W tym przykładzie pokazano, jak umożliwić użytkownikom logowanie za pomocą swojego konta Microsoft, za pomocą projektu programu ASP.NET Core 2.2 utworzone na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="e4cac-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="e4cac-106">Tworzenie aplikacji w portalu dla deweloperów firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="e4cac-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="e4cac-107">Przejdź do [witryna Azure portal — rejestracje aplikacji](https://go.microsoft.com/fwlink/?linkid=2083908) strony i utworzyć lub zaloguj się do konta Microsoft:</span><span class="sxs-lookup"><span data-stu-id="e4cac-107">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="e4cac-108">Jeśli nie masz konta Microsoft, wybierz opcję **utworzyć**.</span><span class="sxs-lookup"><span data-stu-id="e4cac-108">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="e4cac-109">Po zarejestrowaniu się nastąpi przekierowanie do **rejestracje aplikacji** strony:</span><span class="sxs-lookup"><span data-stu-id="e4cac-109">After signing in you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="e4cac-110">Wybierz **nowej rejestracji**</span><span class="sxs-lookup"><span data-stu-id="e4cac-110">Select **New registration**</span></span>
* <span data-ttu-id="e4cac-111">Wprowadź **nazwa**.</span><span class="sxs-lookup"><span data-stu-id="e4cac-111">Enter a **Name**.</span></span>
* <span data-ttu-id="e4cac-112">Wybierz opcję **obsługiwane typy kont**.</span><span class="sxs-lookup"><span data-stu-id="e4cac-112">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="e4cac-113">W obszarze **identyfikator URI przekierowania**, wprowadź adres URL programowania za pomocą `/signin-microsoft` dołączane.</span><span class="sxs-lookup"><span data-stu-id="e4cac-113">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="e4cac-114">Na przykład `https://localhost:44389/signin-microsoft`.</span><span class="sxs-lookup"><span data-stu-id="e4cac-114">For example, `https://localhost:44389/signin-microsoft`.</span></span> <span data-ttu-id="e4cac-115">Schemat uwierzytelniania firmy Microsoft, skonfigurować je później w tym przykładzie będzie automatycznie obsługiwać żądań w `/signin-microsoft` trasy do zaimplementowania przepływu uwierzytelniania OAuth.</span><span class="sxs-lookup"><span data-stu-id="e4cac-115">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="e4cac-116">Wybierz **zarejestrować**</span><span class="sxs-lookup"><span data-stu-id="e4cac-116">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="e4cac-117">Utwórz klucz tajny klienta</span><span class="sxs-lookup"><span data-stu-id="e4cac-117">Create client secret</span></span>

* <span data-ttu-id="e4cac-118">W okienku po lewej stronie wybierz **certyfikaty i klucze tajne**.</span><span class="sxs-lookup"><span data-stu-id="e4cac-118">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="e4cac-119">W obszarze **wpisów tajnych klienta**, wybierz opcję **nowy wpis tajny klienta**</span><span class="sxs-lookup"><span data-stu-id="e4cac-119">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="e4cac-120">Dodaj opis klucza tajnego klienta.</span><span class="sxs-lookup"><span data-stu-id="e4cac-120">Add a description for the client secret.</span></span>
  * <span data-ttu-id="e4cac-121">Wybierz **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="e4cac-121">Select the **Add** button.</span></span>

* <span data-ttu-id="e4cac-122">W obszarze **wpisów tajnych klienta**, skopiuj wartość klucza tajnego klienta.</span><span class="sxs-lookup"><span data-stu-id="e4cac-122">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="e4cac-123">Segmentem identyfikatora URI `/signin-microsoft` jest ustawiony jako domyślny wywołania zwrotnego dostawcy uwierzytelniania firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e4cac-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="e4cac-124">Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania firmy Microsoft za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) klasy.</span><span class="sxs-lookup"><span data-stu-id="e4cac-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="e4cac-125">Store Microsoft klienta identyfikator i klucz tajny klienta</span><span class="sxs-lookup"><span data-stu-id="e4cac-125">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="e4cac-126">Uruchom następujące polecenia, aby bezpiecznie przechowywać `ClientId` i `ClientSecret` przy użyciu [Menedżera klucz tajny](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="e4cac-126">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```console
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="e4cac-127">Link ustawienia poufne, takie jak Microsoft `ClientId` i `ClientSecret` z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="e4cac-127">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="e4cac-128">Do celów tego przykładu, nazwa tokeny `Authentication:Microsoft:ClientId` i `Authentication:Microsoft:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="e4cac-128">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="e4cac-129">Skonfiguruj uwierzytelnianie za pomocą konta Microsoft</span><span class="sxs-lookup"><span data-stu-id="e4cac-129">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="e4cac-130">Dodaj usługę Account Microsoft do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e4cac-130">Add the Microsoft Account service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="e4cac-131">Zobacz [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie Account firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e4cac-131">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="e4cac-132">Może to służyć do żądania różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="e4cac-132">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="e4cac-133">Zaloguj się przy użyciu konta Microsoft</span><span class="sxs-lookup"><span data-stu-id="e4cac-133">Sign in with Microsoft Account</span></span>

<span data-ttu-id="e4cac-134">Uruchom i kliknij przycisk **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="e4cac-134">Run the and click **Log in**.</span></span> <span data-ttu-id="e4cac-135">Pojawi się opcja Zaloguj się przy użyciu konta Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e4cac-135">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="e4cac-136">Po kliknięciu firmy Microsoft są przekierowywane do firmy Microsoft do uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="e4cac-136">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="e4cac-137">Po zarejestrowaniu się przy użyciu Account firmy Microsoft (Jeśli nie zostało to zrobione) użytkownik jest monitowany, aby umożliwić aplikacji dostęp do informacji:</span><span class="sxs-lookup"><span data-stu-id="e4cac-137">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="e4cac-138">Naciśnij pozycję **tak** i nastąpi przekierowanie do witryny sieci web, w którym można ustawić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="e4cac-138">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="e4cac-139">Obecnie zalogowano Cię przy użyciu poświadczeń konta Microsoft:</span><span class="sxs-lookup"><span data-stu-id="e4cac-139">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="e4cac-140">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="e4cac-140">Troubleshooting</span></span>

* <span data-ttu-id="e4cac-141">Jeśli dostawca Account Microsoft przekieruje Cię do strony logowania w błąd, weź pod uwagę błąd tytuł i opis parametrów ciągu zapytania bezpośrednio po `#` (hasztag) w identyfikatorze Uri.</span><span class="sxs-lookup"><span data-stu-id="e4cac-141">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="e4cac-142">Mimo, że komunikat o błędzie wydaje się, że wskazywać na problem z uwierzytelniania firmy Microsoft, Najczęstszą przyczyną jest niezgodne żadnego z identyfikatora Uri aplikacji **identyfikatory URI przekierowań** określony dla **Web** platformy .</span><span class="sxs-lookup"><span data-stu-id="e4cac-142">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="e4cac-143">Jeśli tożsamość nie jest skonfigurowana, wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: Opcja "SignInScheme" musi być podana*.</span><span class="sxs-lookup"><span data-stu-id="e4cac-143">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="e4cac-144">Szablon projektu, używane w tym przykładzie gwarantuje, że odbywa się.</span><span class="sxs-lookup"><span data-stu-id="e4cac-144">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="e4cac-145">Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, zostanie wyświetlony *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu.</span><span class="sxs-lookup"><span data-stu-id="e4cac-145">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="e4cac-146">Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.</span><span class="sxs-lookup"><span data-stu-id="e4cac-146">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4cac-147">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="e4cac-147">Next steps</span></span>

* <span data-ttu-id="e4cac-148">W tym artykule pokazano, jak można uwierzytelniać z firmą Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e4cac-148">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="e4cac-149">Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="e4cac-149">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="e4cac-150">Po możesz opublikować witrynę sieci web do aplikacji sieci web platformy Azure, Utwórz nowy klient wpisów tajnych w portalu dla deweloperów firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e4cac-150">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="e4cac-151">Ustaw `Authentication:Microsoft:ClientId` i `Authentication:Microsoft:ClientSecret` jako ustawienia aplikacji w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="e4cac-151">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="e4cac-152">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="e4cac-152">The configuration system is set up to read keys from environment variables.</span></span>