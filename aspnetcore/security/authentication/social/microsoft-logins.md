---
title: Konfiguracja logowania zewnętrznego konta Microsoft z ASP.NET Core
author: rick-anderson
description: Ten przykład pokazuje integrację konto Microsoft uwierzytelniania użytkowników w istniejącej aplikacji ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/4/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/microsoft-logins
ms.openlocfilehash: ddaae1a25a1dcf167ffae0f24b480e2cde6aca5b
ms.sourcegitcommit: f4cd3828e26e6d549ba8d0c36a17be35ad9e5a51
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74825465"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="3d930-103">Konfiguracja logowania zewnętrznego konta Microsoft z ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3d930-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="3d930-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3d930-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3d930-105">Ten przykład pokazuje, jak umożliwić użytkownikom logowanie się za pomocą konto Microsoft przy użyciu projektu ASP.NET Core 3,0 utworzonego na [poprzedniej stronie](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="3d930-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="3d930-106">Tworzenie aplikacji w portalu dla deweloperów firmy Microsoft</span><span class="sxs-lookup"><span data-stu-id="3d930-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="3d930-107">Dodaj pakiet NuGet [Microsoft. AspNetCore. Authentication. MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) do projektu.</span><span class="sxs-lookup"><span data-stu-id="3d930-107">Add the [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) NuGet package to the project.</span></span>
* <span data-ttu-id="3d930-108">Przejdź do strony [Azure Portal-rejestracje aplikacji](https://go.microsoft.com/fwlink/?linkid=2083908) i Utwórz lub Zaloguj się do konto Microsoft:</span><span class="sxs-lookup"><span data-stu-id="3d930-108">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="3d930-109">Jeśli nie masz konto Microsoft, wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="3d930-109">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="3d930-110">Po zalogowaniu nastąpi przekierowanie do strony **rejestracje aplikacji** :</span><span class="sxs-lookup"><span data-stu-id="3d930-110">After signing in, you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="3d930-111">Wybierz **nową rejestrację**</span><span class="sxs-lookup"><span data-stu-id="3d930-111">Select **New registration**</span></span>
* <span data-ttu-id="3d930-112">Wprowadź **nazwę**.</span><span class="sxs-lookup"><span data-stu-id="3d930-112">Enter a **Name**.</span></span>
* <span data-ttu-id="3d930-113">Wybierz opcję dla **obsługiwanych typów kont**.</span><span class="sxs-lookup"><span data-stu-id="3d930-113">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="3d930-114">W obszarze **Identyfikator URI przekierowania**wprowadź adres URL Twojego projektu, `/signin-microsoft` dołączyć.</span><span class="sxs-lookup"><span data-stu-id="3d930-114">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="3d930-115">Na przykład `https://localhost:5001/signin-microsoft`.</span><span class="sxs-lookup"><span data-stu-id="3d930-115">For example, `https://localhost:5001/signin-microsoft`.</span></span> <span data-ttu-id="3d930-116">Schemat uwierzytelniania firmy Microsoft skonfigurowany w dalszej części tego przykładu będzie automatycznie obsługiwał żądania w ramach trasy `/signin-microsoft` w celu zaimplementowania przepływu OAuth.</span><span class="sxs-lookup"><span data-stu-id="3d930-116">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="3d930-117">Wybierz pozycję **zarejestruj**</span><span class="sxs-lookup"><span data-stu-id="3d930-117">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="3d930-118">Utwórz klucz tajny klienta</span><span class="sxs-lookup"><span data-stu-id="3d930-118">Create client secret</span></span>

* <span data-ttu-id="3d930-119">W lewym okienku wybierz pozycję **certyfikaty & wpisy tajne**.</span><span class="sxs-lookup"><span data-stu-id="3d930-119">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="3d930-120">W obszarze wpisy **tajne klienta**wybierz pozycję **nowy klucz tajny klienta** .</span><span class="sxs-lookup"><span data-stu-id="3d930-120">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="3d930-121">Dodaj opis wpisu tajnego klienta.</span><span class="sxs-lookup"><span data-stu-id="3d930-121">Add a description for the client secret.</span></span>
  * <span data-ttu-id="3d930-122">Wybierz przycisk **Add** (Dodaj).</span><span class="sxs-lookup"><span data-stu-id="3d930-122">Select the **Add** button.</span></span>

* <span data-ttu-id="3d930-123">W obszarze wpisy **tajne klienta**skopiuj wartość klucza tajnego klienta.</span><span class="sxs-lookup"><span data-stu-id="3d930-123">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="3d930-124">Segment identyfikatora URI `/signin-microsoft` jest ustawiany jako domyślne wywołanie zwrotne dostawcy uwierzytelniania firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3d930-124">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="3d930-125">Można zmienić domyślny identyfikator URI wywołania zwrotnego podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania firmy Microsoft za pośrednictwem dziedziczonej właściwości [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) klasy [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) .</span><span class="sxs-lookup"><span data-stu-id="3d930-125">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="3d930-126">Zapisz identyfikator klienta firmy Microsoft i klucz tajny klienta</span><span class="sxs-lookup"><span data-stu-id="3d930-126">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="3d930-127">Uruchom następujące polecenia, aby bezpiecznie przechowywać `ClientId` i `ClientSecret` przy użyciu [Menedżera wpisów tajnych](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="3d930-127">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="3d930-128">Połącz poufne ustawienia, takie jak Microsoft `ClientId` i `ClientSecret` z konfiguracją aplikacji za pomocą [Menedżera wpisów tajnych](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="3d930-128">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="3d930-129">Na potrzeby tego przykładu Nazwij tokeny `Authentication:Microsoft:ClientId` i `Authentication:Microsoft:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="3d930-129">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="3d930-130">Konfigurowanie uwierzytelniania konta Microsoft</span><span class="sxs-lookup"><span data-stu-id="3d930-130">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="3d930-131">Dodaj usługę konta Microsoft do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3d930-131">Add the Microsoft Account service to the `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupMS3x.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="3d930-132">Aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie konta Microsoft, zobacz Dokumentacja interfejsu API [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) .</span><span class="sxs-lookup"><span data-stu-id="3d930-132">For more information about configuration options supported by Microsoft Account authentication, see the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference.</span></span> <span data-ttu-id="3d930-133">Może to służyć do żądania różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="3d930-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="3d930-134">Konto Zaloguj się przy użyciu konta Microsoft</span><span class="sxs-lookup"><span data-stu-id="3d930-134">Sign in with Microsoft Account</span></span>

<span data-ttu-id="3d930-135">Uruchom aplikację i kliknij pozycję **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="3d930-135">Run the app and click **Log in**.</span></span> <span data-ttu-id="3d930-136">Zostanie wyświetlona opcja zalogowania się do firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3d930-136">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="3d930-137">Po kliknięciu firmy Microsoft nastąpi przekierowanie do firmy Microsoft w celu uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="3d930-137">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="3d930-138">Po zalogowaniu się przy użyciu konta Microsoft zostanie wyświetlony monit o zezwolenie aplikacji na dostęp do informacji:</span><span class="sxs-lookup"><span data-stu-id="3d930-138">After signing in with your Microsoft Account, you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="3d930-139">Naciśnij pozycję **tak** . nastąpi przekierowanie z powrotem do witryny sieci Web, w której można ustawić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="3d930-139">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="3d930-140">Jesteś teraz zalogowany przy użyciu poświadczeń firmy Microsoft:</span><span class="sxs-lookup"><span data-stu-id="3d930-140">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="3d930-141">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="3d930-141">Troubleshooting</span></span>

* <span data-ttu-id="3d930-142">Jeśli dostawca kont Microsoft przekieruje Cię do strony błędu logowania, należy zwrócić uwagę na tytuły i opis błędu parametrów zapytania bezpośrednio po `#` (hasztagów) w identyfikatorze URI.</span><span class="sxs-lookup"><span data-stu-id="3d930-142">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="3d930-143">Mimo że zostanie wyświetlony komunikat o błędzie z informacją o problemie z uwierzytelnianiem firmy Microsoft, najbardziej typową przyczyną jest to, że identyfikator URI aplikacji nie pasuje do żadnego z **identyfikatorów URI przekierowania** określonych dla danej platformy **sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="3d930-143">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="3d930-144">Jeśli tożsamość nie jest konfigurowana przez wywołanie `services.AddIdentity` w `ConfigureServices`, próba uwierzytelnienia spowoduje powstanie *argumentu ArgumentException: należy podać opcję "SignInScheme"* .</span><span class="sxs-lookup"><span data-stu-id="3d930-144">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="3d930-145">Szablon projektu używany w tym przykładzie zapewnia, że jest to gotowe.</span><span class="sxs-lookup"><span data-stu-id="3d930-145">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="3d930-146">Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, zostanie wyświetlony *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu.</span><span class="sxs-lookup"><span data-stu-id="3d930-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="3d930-147">Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.</span><span class="sxs-lookup"><span data-stu-id="3d930-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3d930-148">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="3d930-148">Next steps</span></span>

* <span data-ttu-id="3d930-149">W tym artykule pokazano, jak można uwierzytelnić się w firmie Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3d930-149">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="3d930-150">Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="3d930-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="3d930-151">Po opublikowaniu witryny sieci Web w usłudze Azure Web App Utwórz nowe wpisy tajne klienta w portalu dla deweloperów firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3d930-151">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="3d930-152">Ustaw `Authentication:Microsoft:ClientId` i `Authentication:Microsoft:ClientSecret` jako ustawienia aplikacji w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="3d930-152">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="3d930-153">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="3d930-153">The configuration system is set up to read keys from environment variables.</span></span>
