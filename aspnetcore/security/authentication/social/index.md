---
title: Facebook, Google i zewnętrznego dostawcy uwierzytelniania w programie ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono sposób tworzenia platformy ASP.NET Core 2.x aplikacji przy użyciu protokołu OAuth 2.0 przy użyciu dostawcy uwierzytelniania zewnętrznego.
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2019
uid: security/authentication/social/index
ms.openlocfilehash: 8dac8a8a2276388414b6bb1211e970617b001637
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874807"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="fdb8a-103">Facebook, Google i zewnętrznego dostawcy uwierzytelniania w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fdb8a-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="fdb8a-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fdb8a-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fdb8a-105">W tym samouczku pokazano, jak utworzyć aplikację platformy ASP.NET Core 2.2, która umożliwia użytkownikom zalogowanie się przy użyciu protokołu OAuth 2.0 przy użyciu poświadczeń z dostawcy uwierzytelniania zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="fdb8a-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), i [Microsoft](xref:security/authentication/microsoft-logins) dostawców znajdują się w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="fdb8a-107">Innych dostawców są dostępne w innych pakietach przykład [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) i [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="fdb8a-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Ikony mediów społecznościowych usługi Facebook, Twitter, Google, plus i Windows](index/_static/social.png)

<span data-ttu-id="fdb8a-109">Umożliwienie użytkownikom logowania się za pomocą istniejących poświadczeń:</span><span class="sxs-lookup"><span data-stu-id="fdb8a-109">Enabling users to sign in with their existing credentials:</span></span>
* <span data-ttu-id="fdb8a-110">Jest przydatna dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-110">Is convenient for the users.</span></span>
* <span data-ttu-id="fdb8a-111">Przenosi wiele złożoności zarządzania procesu logowania na innej.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-111">Shifts many of the complexities of managing the sign-in process onto a third party.</span></span> 

<span data-ttu-id="fdb8a-112">Aby przykładów jak społecznościowych nazw logowania, może zapewnić konwersje typów ruchu i klienta, zobacz przypadków przez [Facebook](https://www.facebook.com/unsupportedbrowser) i [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="fdb8a-112">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="fdb8a-113">Utwórz nowy projekt platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fdb8a-113">Create a New ASP.NET Core Project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fdb8a-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fdb8a-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fdb8a-115">Utwórz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-115">Create a new project.</span></span>
* <span data-ttu-id="fdb8a-116">Wybierz **aplikacji sieci Web platformy ASP.NET Core** i **dalej**.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-116">Select **ASP.NET Core Web Application** and **Next**.</span></span>
* <span data-ttu-id="fdb8a-117">Podaj **Nazwa projektu** i potwierdzić lub zmienić **lokalizacji**.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-117">Provide a **Project name** and confirm or change the **Location**.</span></span> <span data-ttu-id="fdb8a-118">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-118">Select **Create**.</span></span>
* <span data-ttu-id="fdb8a-119">Wybierz **platformy ASP.NET Core 2.2** na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-119">Select **ASP.NET Core 2.2** in the drop down.</span></span> <span data-ttu-id="fdb8a-120">Wybierz **aplikacji sieci Web** na liście szablonów.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-120">Select **Web Application** in the template list.</span></span>
* <span data-ttu-id="fdb8a-121">W obszarze **uwierzytelniania**, wybierz opcję **zmiany** i ustaw uwierzytelnianie **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-121">Under **Authentication**, select **Change** and set the authentication to **Individual User Accounts**.</span></span> <span data-ttu-id="fdb8a-122">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-122">Select **OK**.</span></span>
* <span data-ttu-id="fdb8a-123">W **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-123">In the **Create a new ASP.NET Core Web Application** window, select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fdb8a-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fdb8a-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="fdb8a-125">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="fdb8a-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="fdb8a-126">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-126">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="fdb8a-127">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="fdb8a-127">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o WebApp1 -au Individual -uld
  code -r WebApp1
  ```

  * <span data-ttu-id="fdb8a-128">`dotnet new` Polecenie tworzy nowy projekt strony Razor w *WebApp1* folderu.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-128">The `dotnet new` command creates a new Razor Pages project in the *WebApp1* folder.</span></span>
  * <span data-ttu-id="fdb8a-129">`-uld` używa LocalDB zamiast bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-129">`-uld` uses LocalDB instead of SQLite.</span></span> <span data-ttu-id="fdb8a-130">Pomiń `-uld` używać bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-130">Omit `-uld` to use SQLite.</span></span>
  * <span data-ttu-id="fdb8a-131">`-au Individual` Tworzy kod dla poszczególnych uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-131">`-au Individual` creates the code for Individual authentication.</span></span>
  * <span data-ttu-id="fdb8a-132">`code` Polecenia otwiera *WebApp1* folderu w nowym wystąpieniu programu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-132">The `code` command opens the *WebApp1* folder in a new instance of Visual Studio Code.</span></span>

* <span data-ttu-id="fdb8a-133">Zostanie wyświetlone okno dialogowe z **"WebApp1" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**</span><span class="sxs-lookup"><span data-stu-id="fdb8a-133">A dialog box appears with **Required assets to build and debug are missing from 'WebApp1'. Add them?**</span></span> <span data-ttu-id="fdb8a-134">Wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-134">Select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fdb8a-135">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fdb8a-135">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fdb8a-136">Wybierz **pliku** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-136">Select **File** > **New Solution**.</span></span>
* <span data-ttu-id="fdb8a-137">Wybierz **platformy .NET Core** > **aplikacji** na pasku bocznym.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-137">Select **.NET Core** > **App** in the sidebar.</span></span> <span data-ttu-id="fdb8a-138">Wybierz **aplikacji sieci Web** szablonu.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-138">Select the **Web Application** template.</span></span> <span data-ttu-id="fdb8a-139">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-139">Select **Next**.</span></span>
* <span data-ttu-id="fdb8a-140">Ustaw **platformę docelową** listy rozwijanej **platformy .NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-140">Set the **Target Framework** drop down to **.NET Core 2.2**.</span></span> <span data-ttu-id="fdb8a-141">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-141">Select **Next**.</span></span>
* <span data-ttu-id="fdb8a-142">Podaj **nazwy projektu**.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-142">Provide a **Project Name**.</span></span> <span data-ttu-id="fdb8a-143">Upewnij się, lub zmień **lokalizacji**.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-143">Confirm or change the **Location**.</span></span> <span data-ttu-id="fdb8a-144">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-144">Select **Create**.</span></span>

---

## <a name="apply-migrations"></a><span data-ttu-id="fdb8a-145">Zastosuj migracji</span><span class="sxs-lookup"><span data-stu-id="fdb8a-145">Apply migrations</span></span>

* <span data-ttu-id="fdb8a-146">Uruchom aplikację i wybierz **zarejestrować** łącza.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-146">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="fdb8a-147">Wprowadź adres e-mail i hasło dla nowego konta, a następnie wybierz **zarejestrować**.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-147">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="fdb8a-148">Postępuj zgodnie z instrukcjami, aby zastosować migracji.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-148">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="fdb8a-149">Użyj SecretManager do przechowywania tokenów przypisany przez dostawców logowania</span><span class="sxs-lookup"><span data-stu-id="fdb8a-149">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="fdb8a-150">Przypisz dostawców społecznościowych logowania **identyfikator aplikacji** i **klucz tajny aplikacji** tokenów podczas procesu rejestracji.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-150">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="fdb8a-151">Dokładne nazwy tokenu zależą od dostawcy.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-151">The exact token names vary by provider.</span></span> <span data-ttu-id="fdb8a-152">Tokeny te reprezentują poświadczenia aplikacja korzysta z dostępu do swojego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-152">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="fdb8a-153">Tokeny stanowią "wpisy tajne", które mogą być połączone z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="fdb8a-153">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="fdb8a-154">Menedżer klucz tajny jest alternatywą bardziej bezpieczne przechowywanie tokenów w pliku konfiguracji, takich jak *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-154">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fdb8a-155">Menedżer klucz tajny jest tylko do celów programowania.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-155">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="fdb8a-156">Możesz przechowywać i chronić Azure testowych i produkcyjnych wpisów tajnych za pomocą [dostawcę konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="fdb8a-156">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="fdb8a-157">Postępuj zgodnie z instrukcjami w [bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie opracowywania w programie ASP.NET Core](xref:security/app-secrets) tematu do przechowywania tokenów przypisany przez każdego dostawcy logowania poniżej.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-157">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="fdb8a-158">Konfigurowanie dostawców logowania, wymagane przez aplikację</span><span class="sxs-lookup"><span data-stu-id="fdb8a-158">Setup login providers required by your application</span></span>

<span data-ttu-id="fdb8a-159">Użyj poniższych tematów, aby skonfigurować aplikację do używania odpowiednich dostawców:</span><span class="sxs-lookup"><span data-stu-id="fdb8a-159">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="fdb8a-160">[Facebook](xref:security/authentication/facebook-logins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="fdb8a-160">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="fdb8a-161">[W usłudze Twitter](xref:security/authentication/twitter-logins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="fdb8a-161">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="fdb8a-162">[Google](xref:security/authentication/google-logins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="fdb8a-162">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="fdb8a-163">[Microsoft](xref:security/authentication/microsoft-logins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="fdb8a-163">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="fdb8a-164">[Inni dostawcy](xref:security/authentication/otherlogins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="fdb8a-164">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="fdb8a-165">Opcjonalnie Ustaw hasło</span><span class="sxs-lookup"><span data-stu-id="fdb8a-165">Optionally set password</span></span>

<span data-ttu-id="fdb8a-166">Po zarejestrowaniu się przy użyciu dostawcy logowania zewnętrznego, nie ma hasła zarejestrowane z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-166">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="fdb8a-167">Pozwala to uniknąć z tworzenia i zapamiętywanie hasła dla tej witryny, ale zapewnia także możesz zależne od dostawcy logowania zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-167">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="fdb8a-168">Jeśli dostawcy logowania zewnętrznego jest niedostępny, nie można zalogować się do witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-168">If the external login provider is unavailable, you won't be able to sign in to the web site.</span></span>

<span data-ttu-id="fdb8a-169">Aby utworzyć hasło i zalogować się przy użyciu ustawioną podczas procesu logowania z zewnętrznych dostawców poczty e-mail:</span><span class="sxs-lookup"><span data-stu-id="fdb8a-169">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="fdb8a-170">Wybierz **Hello &lt;aliasu adresu e-mail&gt;**  link w prawym górnym rogu, aby przejść do **Zarządzaj** widoku.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-170">Select the **Hello &lt;email alias&gt;** link at the top-right corner to navigate to the **Manage** view.</span></span>

![Widok zarządzania aplikacji sieci Web](index/_static/pass1a.png)

* <span data-ttu-id="fdb8a-172">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="fdb8a-172">Select **Create**</span></span>

![Ustawianie strony hasła](index/_static/pass2a.png)

* <span data-ttu-id="fdb8a-174">Ustawione prawidłowe hasło i umożliwia to logowanie za pomocą poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-174">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fdb8a-175">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="fdb8a-175">Next steps</span></span>

* <span data-ttu-id="fdb8a-176">W tym artykule wprowadzono uwierzytelniania zewnętrznego i opisano wymagania wstępne dotyczące dodawania logowań zewnętrznych do aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-176">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="fdb8a-177">Odwołanie do strony właściwe dla dostawcy Konfigurowanie logowania dla dostawcy, wymagane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-177">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="fdb8a-178">Można utrwalić dodatkowe dane dotyczące użytkownika i tokenami dostępu i odświeżania.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-178">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="fdb8a-179">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="fdb8a-179">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
