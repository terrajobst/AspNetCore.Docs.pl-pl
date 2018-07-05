---
title: Facebook, Google i zewnętrznego dostawcy uwierzytelniania w programie ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono sposób tworzenia platformy ASP.NET Core 2.x aplikacji przy użyciu protokołu OAuth 2.0 przy użyciu dostawcy uwierzytelniania zewnętrznego.
ms.author: riande
ms.date: 11/01/2016
uid: security/authentication/social/index
ms.openlocfilehash: b3fbd98215537fad7b283d1bf96ebd259e0b980a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366278"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="902a6-103">Facebook, Google i zewnętrznego dostawcy uwierzytelniania w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="902a6-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="902a6-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="902a6-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="902a6-105">W tym samouczku przedstawiono sposób tworzenia platformy ASP.NET Core 2.x aplikacji, która umożliwia użytkownikom zalogowanie się przy użyciu protokołu OAuth 2.0 przy użyciu poświadczeń z dostawcy uwierzytelniania zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="902a6-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="902a6-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), i [Microsoft](xref:security/authentication/microsoft-logins) dostawców znajdują się w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="902a6-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="902a6-107">Innych dostawców są dostępne w innych pakietach przykład [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) i [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="902a6-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Ikony mediów społecznościowych usługi Facebook, Twitter, Google, plus i Windows](index/_static/social.png)

<span data-ttu-id="902a6-109">Umożliwienie użytkownikom logowania się za pomocą istniejących poświadczeń jest wygodne w przypadku użytkowników i przenosi wiele złożoności zarządzania procesu logowania na innej.</span><span class="sxs-lookup"><span data-stu-id="902a6-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="902a6-110">Aby przykładów jak społecznościowych nazw logowania, może zapewnić konwersje typów ruchu i klienta, zobacz przypadków przez [Facebook](https://www.facebook.com/unsupportedbrowser) i [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="902a6-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="902a6-111">Uwaga: Pakiety przedstawionych w tym miejscu abstrakcji dużym stopniem złożoności przepływu uwierzytelniania OAuth, ale zrozumienie szczegóły mogą okazać się niezbędne podczas rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="902a6-111">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="902a6-112">Wiele zasobów są dostępne; na przykład zobacz [wprowadzenie do protokołu OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) lub [opis protokołu OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span><span class="sxs-lookup"><span data-stu-id="902a6-112">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="902a6-113">Niektóre problemy można rozwiązać, analizując [kod źródłowy platformy ASP.NET Core pakiety dostawcy](https://github.com/aspnet/Security/tree/dev/src).</span><span class="sxs-lookup"><span data-stu-id="902a6-113">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="902a6-114">Utwórz nowy projekt platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="902a6-114">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="902a6-115">W programie Visual Studio 2017, Utwórz nowy projekt z poziomu strony startowej lub za pośrednictwem **Plik > Nowy > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="902a6-115">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="902a6-116">Wybierz **aplikacji sieci Web programu ASP.NET Core** szablonu dostępnego w **Visual C# > .NET Core** kategorii:</span><span class="sxs-lookup"><span data-stu-id="902a6-116">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![Okno dialogowe nowego projektu](index/_static/new-project.png)

* <span data-ttu-id="902a6-118">Naciśnij pozycję **aplikacji sieci Web** i sprawdź **uwierzytelniania** ustawiono **indywidualne konta użytkowników**:</span><span class="sxs-lookup"><span data-stu-id="902a6-118">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Okno dialogowe Nowy aplikacji sieci Web](index/_static/select-project.png)

<span data-ttu-id="902a6-120">Uwaga: Ten samouczek dotyczy wersji platformy ASP.NET Core 2.0 SDK, które można wybrać w górnej części kreatora.</span><span class="sxs-lookup"><span data-stu-id="902a6-120">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="902a6-121">Zastosuj migracji</span><span class="sxs-lookup"><span data-stu-id="902a6-121">Apply migrations</span></span>

* <span data-ttu-id="902a6-122">Uruchom aplikację i wybierz **Zaloguj** łącza.</span><span class="sxs-lookup"><span data-stu-id="902a6-122">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="902a6-123">Wybierz **Zarejestruj się jako nowy użytkownik** łącza.</span><span class="sxs-lookup"><span data-stu-id="902a6-123">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="902a6-124">Wprowadź adres e-mail i hasło dla nowego konta, a następnie wybierz **zarejestrować**.</span><span class="sxs-lookup"><span data-stu-id="902a6-124">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="902a6-125">Postępuj zgodnie z instrukcjami, aby zastosować migracji.</span><span class="sxs-lookup"><span data-stu-id="902a6-125">Follow the instructions to apply migrations.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="902a6-126">Wymagaj protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="902a6-126">Require SSL</span></span>

<span data-ttu-id="902a6-127">OAuth 2.0 wymaga użycia protokołu SSL do uwierzytelniania za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="902a6-127">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="902a6-128">Uwaga: Projekty utworzone za pomocą **aplikacji sieci Web** lub **interfejsu API sieci Web** szablony projektów dla platformy ASP.NET Core 2.x są konfigurowane automatycznie w celu włączenia protokołu SSL, a następnie uruchom za pomocą adresu URL https, jeśli **indywidualnych Konta użytkowników** została zaznaczona opcja **okno dialogowe Zmień uwierzytelnianie** w Kreatorze projektu, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="902a6-128">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="902a6-129">Wymagaj protokołu SSL w lokacji, wykonując kroki opisane w [Wymuszanie protokołu SSL w aplikacji ASP.NET Core](xref:security/enforcing-ssl) tematu.</span><span class="sxs-lookup"><span data-stu-id="902a6-129">Require SSL on your site by following the steps in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="902a6-130">Użyj SecretManager do przechowywania tokenów przypisany przez dostawców logowania</span><span class="sxs-lookup"><span data-stu-id="902a6-130">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="902a6-131">Przypisz dostawców społecznościowych logowania **identyfikator aplikacji** i **klucz tajny aplikacji** tokenów podczas procesu rejestracji.</span><span class="sxs-lookup"><span data-stu-id="902a6-131">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="902a6-132">Dokładne nazwy tokenu zależą od dostawcy.</span><span class="sxs-lookup"><span data-stu-id="902a6-132">The exact token names vary by provider.</span></span> <span data-ttu-id="902a6-133">Tokeny te reprezentują poświadczenia aplikacja korzysta z dostępu do swojego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="902a6-133">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="902a6-134">Tokeny stanowią "wpisy tajne", które mogą być połączone z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="902a6-134">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="902a6-135">Menedżer klucz tajny jest alternatywą bardziej bezpieczne przechowywanie tokenów w pliku konfiguracji, takich jak *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="902a6-135">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="902a6-136">Menedżer klucz tajny jest tylko do celów programowania.</span><span class="sxs-lookup"><span data-stu-id="902a6-136">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="902a6-137">Możesz przechowywać i chronić Azure testowych i produkcyjnych wpisów tajnych za pomocą [dostawcę konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="902a6-137">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="902a6-138">Postępuj zgodnie z instrukcjami w [bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie opracowywania w programie ASP.NET Core](xref:security/app-secrets) tematu do przechowywania tokenów przypisany przez każdego dostawcy logowania poniżej.</span><span class="sxs-lookup"><span data-stu-id="902a6-138">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="902a6-139">Konfigurowanie dostawców logowania, wymagane przez aplikację</span><span class="sxs-lookup"><span data-stu-id="902a6-139">Setup login providers required by your application</span></span>

<span data-ttu-id="902a6-140">Użyj poniższych tematów, aby skonfigurować aplikację do używania odpowiednich dostawców:</span><span class="sxs-lookup"><span data-stu-id="902a6-140">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="902a6-141">[Facebook](xref:security/authentication/facebook-logins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="902a6-141">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="902a6-142">[W usłudze Twitter](xref:security/authentication/twitter-logins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="902a6-142">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="902a6-143">[Google](xref:security/authentication/google-logins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="902a6-143">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="902a6-144">[Microsoft](xref:security/authentication/microsoft-logins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="902a6-144">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="902a6-145">[Inni dostawcy](xref:security/authentication/otherlogins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="902a6-145">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](~/includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="902a6-146">Opcjonalnie Ustaw hasło</span><span class="sxs-lookup"><span data-stu-id="902a6-146">Optionally set password</span></span>

<span data-ttu-id="902a6-147">Po zarejestrowaniu się przy użyciu dostawcy logowania zewnętrznego, nie ma hasła zarejestrowane z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="902a6-147">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="902a6-148">Pozwala to uniknąć z tworzenia i zapamiętywanie hasła dla tej witryny, ale zapewnia także możesz zależne od dostawcy logowania zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="902a6-148">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="902a6-149">Jeśli dostawcy logowania zewnętrznego jest niedostępny, nie można zalogować się do witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="902a6-149">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="902a6-150">Aby utworzyć hasło i zalogować się przy użyciu ustawioną podczas procesu logowania z zewnętrznych dostawców poczty e-mail:</span><span class="sxs-lookup"><span data-stu-id="902a6-150">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="902a6-151">Naciśnij pozycję **Hello &lt;aliasu adresu e-mail&gt;**  link w prawym górnym rogu, aby przejść do **Zarządzaj** widoku.</span><span class="sxs-lookup"><span data-stu-id="902a6-151">Tap the **Hello &lt;email alias&gt;** link at the top right corner to navigate to the **Manage** view.</span></span>

![Widok zarządzania aplikacji sieci Web](index/_static/pass1a.png)

* <span data-ttu-id="902a6-153">Naciśnij pozycję **tworzenie**</span><span class="sxs-lookup"><span data-stu-id="902a6-153">Tap **Create**</span></span>

![Ustawianie strony hasła](index/_static/pass2a.png)

* <span data-ttu-id="902a6-155">Ustawione prawidłowe hasło i umożliwia to logowanie za pomocą poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="902a6-155">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="902a6-156">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="902a6-156">Next steps</span></span>

* <span data-ttu-id="902a6-157">W tym artykule wprowadzono uwierzytelniania zewnętrznego i opisano wymagania wstępne dotyczące dodawania logowań zewnętrznych do aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="902a6-157">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="902a6-158">Odwołanie do strony właściwe dla dostawcy Konfigurowanie logowania dla dostawcy, wymagane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="902a6-158">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
