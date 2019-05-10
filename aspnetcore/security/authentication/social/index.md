---
title: Facebook, Google i zewnętrznego dostawcy uwierzytelniania w programie ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono sposób tworzenia platformy ASP.NET Core 2.x aplikacji przy użyciu protokołu OAuth 2.0 przy użyciu dostawcy uwierzytelniania zewnętrznego.
ms.author: riande
ms.custom: mvc
ms.date: 4/19/2019
uid: security/authentication/social/index
ms.openlocfilehash: e2d68ac93bdcfa2fc015e8447ea38626787cdb02
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65451043"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="25a75-103">Facebook, Google i zewnętrznego dostawcy uwierzytelniania w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25a75-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="25a75-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="25a75-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="25a75-105">W tym samouczku pokazano, jak utworzyć aplikację platformy ASP.NET Core 2.2, która umożliwia użytkownikom zalogowanie się przy użyciu protokołu OAuth 2.0 przy użyciu poświadczeń z dostawcy uwierzytelniania zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="25a75-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="25a75-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), i [Microsoft](xref:security/authentication/microsoft-logins) dostawców znajdują się w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="25a75-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="25a75-107">Innych dostawców są dostępne w innych pakietach przykład [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) i [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="25a75-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Ikony mediów społecznościowych usługi Facebook, Twitter, Google, plus i Windows](index/_static/social.png)

<span data-ttu-id="25a75-109">Umożliwienie użytkownikom logowania się za pomocą istniejących poświadczeń:</span><span class="sxs-lookup"><span data-stu-id="25a75-109">Enabling users to sign in with their existing credentials:</span></span>
* <span data-ttu-id="25a75-110">Jest przydatna dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="25a75-110">Is convenient for the users.</span></span>
* <span data-ttu-id="25a75-111">Przenosi wiele złożoności zarządzania procesu logowania na innej.</span><span class="sxs-lookup"><span data-stu-id="25a75-111">Shifts many of the complexities of managing the sign-in process onto a third party.</span></span> 

<span data-ttu-id="25a75-112">Aby przykładów jak społecznościowych nazw logowania, może zapewnić konwersje typów ruchu i klienta, zobacz przypadków przez [Facebook](https://www.facebook.com/unsupportedbrowser) i [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="25a75-112">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="25a75-113">Utwórz nowy projekt platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25a75-113">Create a New ASP.NET Core Project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="25a75-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="25a75-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="25a75-115">W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="25a75-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="25a75-116">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="25a75-116">Create a new ASP.NET Core Web Application.</span></span>
* <span data-ttu-id="25a75-117">Wybierz **platformy ASP.NET Core 2.2** w listy rozwijanej, a następnie wybierz pozycję **aplikacji sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="25a75-117">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>
* <span data-ttu-id="25a75-118">Wybierz **Zmień uwierzytelnianie** i uwierzytelniania zestawu **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="25a75-118">Select **Change Authentication** and set authentication to **Individual User Accounts**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="25a75-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="25a75-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="25a75-120">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="25a75-120">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="25a75-121">Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="25a75-121">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="25a75-122">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="25a75-122">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o WebApp1 -au Individual -uld
  code -r WebApp1
  ```

  * <span data-ttu-id="25a75-123">`dotnet new` Polecenie tworzy nowy projekt strony Razor w *WebApp1* folderu.</span><span class="sxs-lookup"><span data-stu-id="25a75-123">The `dotnet new` command creates a new Razor Pages project in the *WebApp1* folder.</span></span>
  * <span data-ttu-id="25a75-124">`-uld` używa LocalDB zamiast bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="25a75-124">`-uld` uses LocalDB instead of SQLite.</span></span> <span data-ttu-id="25a75-125">Pomiń `-uld` używać bazy danych SQLite.</span><span class="sxs-lookup"><span data-stu-id="25a75-125">Omit `-uld` to use SQLite.</span></span>
  * <span data-ttu-id="25a75-126">`-au Individual` Tworzy kod dla poszczególnych uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="25a75-126">`-au Individual` creates the code for Individual authentication.</span></span>
  * <span data-ttu-id="25a75-127">`code` Polecenia otwiera *WebApp1* folderu w nowym wystąpieniu programu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="25a75-127">The `code` command opens the *WebApp1* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="25a75-128">Zostanie wyświetlone okno dialogowe z **"WebApp1" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**</span><span class="sxs-lookup"><span data-stu-id="25a75-128">A dialog box appears with **Required assets to build and debug are missing from 'WebApp1'. Add them?**</span></span>

* <span data-ttu-id="25a75-129">Wybierz **tak**</span><span class="sxs-lookup"><span data-stu-id="25a75-129">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="25a75-130">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="25a75-130">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="25a75-131">W terminalu uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="25a75-131">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o WebApp1 -au Individual
```

<span data-ttu-id="25a75-132">Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do utworzenia projektu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="25a75-132">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="25a75-133">Otwórz projekt</span><span class="sxs-lookup"><span data-stu-id="25a75-133">Open the project</span></span>

<span data-ttu-id="25a75-134">Z programu Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz pozycję *WebApp1.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="25a75-134">From Visual Studio, select **File > Open**, and then select the *WebApp1.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="apply-migrations"></a><span data-ttu-id="25a75-135">Zastosuj migracji</span><span class="sxs-lookup"><span data-stu-id="25a75-135">Apply migrations</span></span>

* <span data-ttu-id="25a75-136">Uruchom aplikację i wybierz **zarejestrować** łącza.</span><span class="sxs-lookup"><span data-stu-id="25a75-136">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="25a75-137">Wprowadź adres e-mail i hasło dla nowego konta, a następnie wybierz **zarejestrować**.</span><span class="sxs-lookup"><span data-stu-id="25a75-137">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="25a75-138">Postępuj zgodnie z instrukcjami, aby zastosować migracji.</span><span class="sxs-lookup"><span data-stu-id="25a75-138">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="25a75-139">Użyj SecretManager do przechowywania tokenów przypisany przez dostawców logowania</span><span class="sxs-lookup"><span data-stu-id="25a75-139">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="25a75-140">Przypisz dostawców społecznościowych logowania **identyfikator aplikacji** i **klucz tajny aplikacji** tokenów podczas procesu rejestracji.</span><span class="sxs-lookup"><span data-stu-id="25a75-140">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="25a75-141">Dokładne nazwy tokenu zależą od dostawcy.</span><span class="sxs-lookup"><span data-stu-id="25a75-141">The exact token names vary by provider.</span></span> <span data-ttu-id="25a75-142">Tokeny te reprezentują poświadczenia aplikacja korzysta z dostępu do swojego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="25a75-142">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="25a75-143">Tokeny stanowią "wpisy tajne", które mogą być połączone z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="25a75-143">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="25a75-144">Menedżer klucz tajny jest alternatywą bardziej bezpieczne przechowywanie tokenów w pliku konfiguracji, takich jak *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="25a75-144">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="25a75-145">Menedżer klucz tajny jest tylko do celów programowania.</span><span class="sxs-lookup"><span data-stu-id="25a75-145">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="25a75-146">Możesz przechowywać i chronić Azure testowych i produkcyjnych wpisów tajnych za pomocą [dostawcę konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="25a75-146">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="25a75-147">Postępuj zgodnie z instrukcjami w [bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie opracowywania w programie ASP.NET Core](xref:security/app-secrets) tematu do przechowywania tokenów przypisany przez każdego dostawcy logowania poniżej.</span><span class="sxs-lookup"><span data-stu-id="25a75-147">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="25a75-148">Konfigurowanie dostawców logowania, wymagane przez aplikację</span><span class="sxs-lookup"><span data-stu-id="25a75-148">Setup login providers required by your application</span></span>

<span data-ttu-id="25a75-149">Użyj poniższych tematów, aby skonfigurować aplikację do używania odpowiednich dostawców:</span><span class="sxs-lookup"><span data-stu-id="25a75-149">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="25a75-150">[Facebook](xref:security/authentication/facebook-logins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="25a75-150">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="25a75-151">[W usłudze Twitter](xref:security/authentication/twitter-logins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="25a75-151">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="25a75-152">[Google](xref:security/authentication/google-logins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="25a75-152">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="25a75-153">[Microsoft](xref:security/authentication/microsoft-logins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="25a75-153">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="25a75-154">[Inni dostawcy](xref:security/authentication/otherlogins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="25a75-154">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="25a75-155">Opcjonalnie Ustaw hasło</span><span class="sxs-lookup"><span data-stu-id="25a75-155">Optionally set password</span></span>

<span data-ttu-id="25a75-156">Po zarejestrowaniu się przy użyciu dostawcy logowania zewnętrznego, nie ma hasła zarejestrowane z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="25a75-156">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="25a75-157">Pozwala to uniknąć z tworzenia i zapamiętywanie hasła dla tej witryny, ale zapewnia także możesz zależne od dostawcy logowania zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="25a75-157">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="25a75-158">Jeśli dostawcy logowania zewnętrznego jest niedostępny, nie można zalogować się do witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="25a75-158">If the external login provider is unavailable, you won't be able to sign in to the web site.</span></span>

<span data-ttu-id="25a75-159">Aby utworzyć hasło i zalogować się przy użyciu ustawioną podczas procesu logowania z zewnętrznych dostawców poczty e-mail:</span><span class="sxs-lookup"><span data-stu-id="25a75-159">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="25a75-160">Wybierz **Hello &lt;aliasu adresu e-mail&gt;**  link w prawym górnym rogu, aby przejść do **Zarządzaj** widoku.</span><span class="sxs-lookup"><span data-stu-id="25a75-160">Select the **Hello &lt;email alias&gt;** link at the top-right corner to navigate to the **Manage** view.</span></span>

![Widok zarządzania aplikacji sieci Web](index/_static/pass1a.png)

* <span data-ttu-id="25a75-162">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="25a75-162">Select **Create**</span></span>

![Ustawianie strony hasła](index/_static/pass2a.png)

* <span data-ttu-id="25a75-164">Ustawione prawidłowe hasło i umożliwia to logowanie za pomocą poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="25a75-164">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25a75-165">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="25a75-165">Next steps</span></span>

* <span data-ttu-id="25a75-166">W tym artykule wprowadzono uwierzytelniania zewnętrznego i opisano wymagania wstępne dotyczące dodawania logowań zewnętrznych do aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="25a75-166">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="25a75-167">Odwołanie do strony właściwe dla dostawcy Konfigurowanie logowania dla dostawcy, wymagane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="25a75-167">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="25a75-168">Można utrwalić dodatkowe dane dotyczące użytkownika i tokenami dostępu i odświeżania.</span><span class="sxs-lookup"><span data-stu-id="25a75-168">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="25a75-169">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="25a75-169">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
