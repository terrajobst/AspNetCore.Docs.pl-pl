---
title: Uwierzytelnianie w serwisach Facebook, Google i dostawcy zewnętrznym w ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono sposób tworzenia aplikacji ASP.NET Core 2. x przy użyciu protokołu OAuth 2,0 z zewnętrznymi dostawcami uwierzytelniania.
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2019
uid: security/authentication/social/index
ms.openlocfilehash: edaf9eeaf02879b2f7816bab0eb373a7de640c05
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082505"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="1f899-103">Uwierzytelnianie w serwisach Facebook, Google i dostawcy zewnętrznym w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1f899-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="1f899-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1f899-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1f899-105">W tym samouczku przedstawiono sposób tworzenia aplikacji ASP.NET Core 2,2, która umożliwia użytkownikom logowanie się przy użyciu uwierzytelniania OAuth 2,0 z poświadczeniami od zewnętrznych dostawców uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="1f899-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="1f899-106">Dostawcy [serwisu Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins)i [Microsoft](xref:security/authentication/microsoft-logins) są pokryte w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="1f899-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="1f899-107">Inni dostawcy są dostępni w pakietach innych firm, takich jak [ASPNET. Security. OAuth. Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) i [ASPNET. Security. OpenID Connect. Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="1f899-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Ikony mediów społecznościowych dla usług Facebook, Twitter, Google Plus i Windows](index/_static/social.png)

<span data-ttu-id="1f899-109">Umożliwienie użytkownikom logowania się przy użyciu istniejących poświadczeń:</span><span class="sxs-lookup"><span data-stu-id="1f899-109">Enabling users to sign in with their existing credentials:</span></span>
* <span data-ttu-id="1f899-110">Jest wygodna dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="1f899-110">Is convenient for the users.</span></span>
* <span data-ttu-id="1f899-111">Przenosi wiele kompleksów związanych z zarządzaniem procesem logowania na stronie trzeciej.</span><span class="sxs-lookup"><span data-stu-id="1f899-111">Shifts many of the complexities of managing the sign-in process onto a third party.</span></span> 

<span data-ttu-id="1f899-112">Aby zapoznać się z przykładami sposobu, w jaki nazwy logowania społecznościowe mogą na przykład przeanalizować [ruch i konwersje](https://dev.twitter.com/resources/case-studies)klientów, zobacz temat analizy przypadków według [serwisu Facebook](https://www.facebook.com/unsupportedbrowser)</span><span class="sxs-lookup"><span data-stu-id="1f899-112">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="1f899-113">Utwórz nowy projekt ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1f899-113">Create a New ASP.NET Core Project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1f899-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1f899-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1f899-115">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="1f899-115">Create a new project.</span></span>
* <span data-ttu-id="1f899-116">Wybierz pozycję **ASP.NET Core aplikacja sieci Web** i przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="1f899-116">Select **ASP.NET Core Web Application** and **Next**.</span></span>
* <span data-ttu-id="1f899-117">Podaj **nazwę projektu** i Potwierdź lub Zmień **lokalizację**.</span><span class="sxs-lookup"><span data-stu-id="1f899-117">Provide a **Project name** and confirm or change the **Location**.</span></span> <span data-ttu-id="1f899-118">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="1f899-118">Select **Create**.</span></span>
* <span data-ttu-id="1f899-119">Na liście rozwijanej wybierz pozycję **ASP.NET Core 2,2** .</span><span class="sxs-lookup"><span data-stu-id="1f899-119">Select **ASP.NET Core 2.2** in the drop down.</span></span> <span data-ttu-id="1f899-120">Wybierz pozycję **aplikacja sieci Web** na liście szablon.</span><span class="sxs-lookup"><span data-stu-id="1f899-120">Select **Web Application** in the template list.</span></span>
* <span data-ttu-id="1f899-121">W obszarze **uwierzytelnianie**wybierz opcję **Zmień** i ustaw uwierzytelnianie na **konta poszczególnych użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="1f899-121">Under **Authentication**, select **Change** and set the authentication to **Individual User Accounts**.</span></span> <span data-ttu-id="1f899-122">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="1f899-122">Select **OK**.</span></span>
* <span data-ttu-id="1f899-123">W oknie **Tworzenie nowej ASP.NET Core aplikacji sieci Web** wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="1f899-123">In the **Create a new ASP.NET Core Web Application** window, select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1f899-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1f899-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="1f899-125">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="1f899-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="1f899-126">Zmień katalogi (`cd`) na folder, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="1f899-126">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="1f899-127">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="1f899-127">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o WebApp1 -au Individual -uld
  code -r WebApp1
  ```

  * <span data-ttu-id="1f899-128">Polecenie tworzy nowy projekt Razor Pages w folderze WebApp1. `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="1f899-128">The `dotnet new` command creates a new Razor Pages project in the *WebApp1* folder.</span></span>
  * <span data-ttu-id="1f899-129">`-uld`używa LocalDB zamiast oprogramowania SQLite.</span><span class="sxs-lookup"><span data-stu-id="1f899-129">`-uld` uses LocalDB instead of SQLite.</span></span> <span data-ttu-id="1f899-130">Pomiń `-uld` korzystanie z oprogramowania SQLite.</span><span class="sxs-lookup"><span data-stu-id="1f899-130">Omit `-uld` to use SQLite.</span></span>
  * <span data-ttu-id="1f899-131">`-au Individual`Tworzy kod dla indywidualnego uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="1f899-131">`-au Individual` creates the code for Individual authentication.</span></span>
  * <span data-ttu-id="1f899-132">Polecenie otwiera folder WebApp1 w nowym wystąpieniu Visual Studio Code. `code`</span><span class="sxs-lookup"><span data-stu-id="1f899-132">The `code` command opens the *WebApp1* folder in a new instance of Visual Studio Code.</span></span>

* <span data-ttu-id="1f899-133">Zostanie wyświetlone okno dialogowe z **wymaganymi zasobami do kompilowania i debugowania brakuje w "WebApp1". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="1f899-133">A dialog box appears with **Required assets to build and debug are missing from 'WebApp1'. Add them?**</span></span> <span data-ttu-id="1f899-134">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="1f899-134">Select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1f899-135">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="1f899-135">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1f899-136">Wybierz **pliku** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="1f899-136">Select **File** > **New Solution**.</span></span>
* <span data-ttu-id="1f899-137">Na pasku bocznym wybierz pozycję**aplikacja** **.NET Core** > .</span><span class="sxs-lookup"><span data-stu-id="1f899-137">Select **.NET Core** > **App** in the sidebar.</span></span> <span data-ttu-id="1f899-138">Wybierz szablon **aplikacji sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="1f899-138">Select the **Web Application** template.</span></span> <span data-ttu-id="1f899-139">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="1f899-139">Select **Next**.</span></span>
* <span data-ttu-id="1f899-140">Ustaw listę rozwijaną **platformy docelowej** na **platformę .NET Core 2,2**.</span><span class="sxs-lookup"><span data-stu-id="1f899-140">Set the **Target Framework** drop down to **.NET Core 2.2**.</span></span> <span data-ttu-id="1f899-141">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="1f899-141">Select **Next**.</span></span>
* <span data-ttu-id="1f899-142">Podaj **nazwę projektu**.</span><span class="sxs-lookup"><span data-stu-id="1f899-142">Provide a **Project Name**.</span></span> <span data-ttu-id="1f899-143">Potwierdź lub Zmień **lokalizację**.</span><span class="sxs-lookup"><span data-stu-id="1f899-143">Confirm or change the **Location**.</span></span> <span data-ttu-id="1f899-144">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="1f899-144">Select **Create**.</span></span>

---

## <a name="apply-migrations"></a><span data-ttu-id="1f899-145">Zastosuj migracje</span><span class="sxs-lookup"><span data-stu-id="1f899-145">Apply migrations</span></span>

* <span data-ttu-id="1f899-146">Uruchom aplikację i wybierz łącze **zarejestruj** .</span><span class="sxs-lookup"><span data-stu-id="1f899-146">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="1f899-147">Wprowadź adres e-mail i hasło dla nowego konta, a następnie wybierz pozycję **zarejestruj**.</span><span class="sxs-lookup"><span data-stu-id="1f899-147">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="1f899-148">Postępuj zgodnie z instrukcjami, aby zastosować migracje.</span><span class="sxs-lookup"><span data-stu-id="1f899-148">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="1f899-149">Używanie klucza tajnego do przechowywania tokenów przypisanych przez dostawców logowania</span><span class="sxs-lookup"><span data-stu-id="1f899-149">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="1f899-150">Dostawcy logowania społecznościowego przypisują **tokeny** i **identyfikatory aplikacji** podczas procesu rejestracji.</span><span class="sxs-lookup"><span data-stu-id="1f899-150">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="1f899-151">Dokładne nazwy tokenów różnią się w zależności od dostawcy.</span><span class="sxs-lookup"><span data-stu-id="1f899-151">The exact token names vary by provider.</span></span> <span data-ttu-id="1f899-152">Te tokeny reprezentują poświadczenia używane przez aplikację w celu uzyskania dostępu do interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="1f899-152">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="1f899-153">Tokeny stanowią "wpisy tajne", które mogą być połączone z konfiguracją aplikacji za pomocą [Menedżera wpisów tajnych](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="1f899-153">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="1f899-154">Program Secret Manager jest bezpieczniejszym rozwiązaniem do przechowywania tokenów w pliku konfiguracyjnym, takim jak *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="1f899-154">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1f899-155">Menedżer wpisów tajnych służy tylko do celów deweloperskich.</span><span class="sxs-lookup"><span data-stu-id="1f899-155">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="1f899-156">Za pomocą [dostawcy konfiguracji Azure Key Vault](xref:security/key-vault-configuration)można przechowywać i chronić wpisy tajne środowiska Azure test i produkcyjne.</span><span class="sxs-lookup"><span data-stu-id="1f899-156">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="1f899-157">Postępuj zgodnie z instrukcjami [dotyczącymi bezpiecznego przechowywania wpisów tajnych aplikacji podczas opracowywania w ASP.NET Core](xref:security/app-secrets) tematu, aby przechowywać tokeny przypisane przez każdego dostawcę logowania poniżej.</span><span class="sxs-lookup"><span data-stu-id="1f899-157">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="1f899-158">Konfigurowanie dostawców logowania wymaganych przez aplikację</span><span class="sxs-lookup"><span data-stu-id="1f899-158">Setup login providers required by your application</span></span>

<span data-ttu-id="1f899-159">Poniższe tematy zawierają informacje dotyczące konfigurowania aplikacji do korzystania z odpowiednich dostawców:</span><span class="sxs-lookup"><span data-stu-id="1f899-159">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="1f899-160">Instrukcje [serwisu Facebook](xref:security/authentication/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="1f899-160">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="1f899-161">Instrukcje dla usługi [Twitter](xref:security/authentication/twitter-logins)</span><span class="sxs-lookup"><span data-stu-id="1f899-161">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="1f899-162">Instrukcje [Google](xref:security/authentication/google-logins)</span><span class="sxs-lookup"><span data-stu-id="1f899-162">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="1f899-163">Instrukcje [firmy Microsoft](xref:security/authentication/microsoft-logins)</span><span class="sxs-lookup"><span data-stu-id="1f899-163">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="1f899-164">[Inne](xref:security/authentication/otherlogins) instrukcje dotyczące dostawcy</span><span class="sxs-lookup"><span data-stu-id="1f899-164">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="1f899-165">Opcjonalnie Ustaw hasło</span><span class="sxs-lookup"><span data-stu-id="1f899-165">Optionally set password</span></span>

<span data-ttu-id="1f899-166">Gdy zarejestrujesz się przy użyciu zewnętrznego dostawcy logowania, nie masz hasła zarejestrowanego w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1f899-166">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="1f899-167">Pozwala to uniknąć tworzenia i zapamiętywania hasła dla lokacji, ale również od zewnętrznego dostawcy logowania.</span><span class="sxs-lookup"><span data-stu-id="1f899-167">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="1f899-168">Jeśli dostawca logowania zewnętrznego jest niedostępny, nie będzie można zalogować się do witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1f899-168">If the external login provider is unavailable, you won't be able to sign in to the web site.</span></span>

<span data-ttu-id="1f899-169">Aby utworzyć hasło i zalogować się przy użyciu poczty e-mail, która została ustawiona w procesie logowania z zewnętrznymi dostawcami:</span><span class="sxs-lookup"><span data-stu-id="1f899-169">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="1f899-170">Wybierz link **Hello &lt;email alias&gt;**  w prawym górnym rogu, aby przejść do widoku **zarządzania** .</span><span class="sxs-lookup"><span data-stu-id="1f899-170">Select the **Hello &lt;email alias&gt;** link at the top-right corner to navigate to the **Manage** view.</span></span>

![Widok zarządzania aplikacjami sieci Web](index/_static/pass1a.png)

* <span data-ttu-id="1f899-172">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="1f899-172">Select **Create**</span></span>

![Ustawianie strony hasła](index/_static/pass2a.png)

* <span data-ttu-id="1f899-174">Ustaw prawidłowe hasło i możesz go użyć do zalogowania się za pomocą poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="1f899-174">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f899-175">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="1f899-175">Next steps</span></span>

* <span data-ttu-id="1f899-176">W tym artykule wprowadzono uwierzytelnianie zewnętrzne i wyjaśniono wymagania wstępne wymagane do dodania zewnętrznych logowań do aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1f899-176">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="1f899-177">Odwołuje się do stron specyficznych dla dostawcy, aby skonfigurować logowania dla dostawców wymaganych przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="1f899-177">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="1f899-178">Możesz chcieć utrzymać dodatkowe dane dotyczące użytkownika oraz tokeny dostępu i odświeżania.</span><span class="sxs-lookup"><span data-stu-id="1f899-178">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="1f899-179">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="1f899-179">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
