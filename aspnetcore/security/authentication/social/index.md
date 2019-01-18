---
title: Facebook, Google i zewnętrznego dostawcy uwierzytelniania w programie ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono sposób tworzenia platformy ASP.NET Core 2.x aplikacji przy użyciu protokołu OAuth 2.0 przy użyciu dostawcy uwierzytelniania zewnętrznego.
ms.author: riande
ms.custom: mvc
ms.date: 1/19/2019
uid: security/authentication/social/index
ms.openlocfilehash: 48dd8b772234ff18158423a36ed1716102bc2f31
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396145"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="7b73e-103">Facebook, Google i zewnętrznego dostawcy uwierzytelniania w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7b73e-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="7b73e-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7b73e-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7b73e-105">W tym samouczku pokazano, jak utworzyć aplikację platformy ASP.NET Core 2.2, która umożliwia użytkownikom zalogowanie się przy użyciu protokołu OAuth 2.0 przy użyciu poświadczeń z dostawcy uwierzytelniania zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="7b73e-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="7b73e-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), i [Microsoft](xref:security/authentication/microsoft-logins) dostawców znajdują się w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="7b73e-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="7b73e-107">Innych dostawców są dostępne w innych pakietach przykład [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) i [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="7b73e-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Ikony mediów społecznościowych usługi Facebook, Twitter, Google, plus i Windows](index/_static/social.png)

<span data-ttu-id="7b73e-109">Umożliwienie użytkownikom logowania się za pomocą istniejących poświadczeń jest wygodne w przypadku użytkowników i przenosi wiele złożoności zarządzania procesu logowania na innej.</span><span class="sxs-lookup"><span data-stu-id="7b73e-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="7b73e-110">Aby przykładów jak społecznościowych nazw logowania, może zapewnić konwersje typów ruchu i klienta, zobacz przypadków przez [Facebook](https://www.facebook.com/unsupportedbrowser) i [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="7b73e-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="7b73e-111">Utwórz nowy projekt platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7b73e-111">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="7b73e-112">W programie Visual Studio 2017, Utwórz nowy projekt z poziomu strony startowej lub za pośrednictwem **pliku** > **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="7b73e-112">In Visual Studio 2017, create a new project from the Start Page, or via **File** > **New** > **Project**.</span></span>

* <span data-ttu-id="7b73e-113">Wybierz **aplikacji sieci Web programu ASP.NET Core** szablonu dostępnego w **Visual C#**   >  **platformy .NET Core** kategorii:</span><span class="sxs-lookup"><span data-stu-id="7b73e-113">Select the **ASP.NET Core Web Application** template available in the **Visual C#** > **.NET Core** category:</span></span>
* <span data-ttu-id="7b73e-114">Wybierz **Zmień uwierzytelnianie** i uwierzytelniania zestawu **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="7b73e-114">Select **Change Authentication** and set authentication to **Individual User Accounts**.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="7b73e-115">Zastosuj migracji</span><span class="sxs-lookup"><span data-stu-id="7b73e-115">Apply migrations</span></span>

* <span data-ttu-id="7b73e-116">Uruchom aplikację i wybierz **zarejestrować** łącza.</span><span class="sxs-lookup"><span data-stu-id="7b73e-116">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="7b73e-117">Wprowadź adres e-mail i hasło dla nowego konta, a następnie wybierz **zarejestrować**.</span><span class="sxs-lookup"><span data-stu-id="7b73e-117">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="7b73e-118">Postępuj zgodnie z instrukcjami, aby zastosować migracji.</span><span class="sxs-lookup"><span data-stu-id="7b73e-118">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="7b73e-119">Użyj SecretManager do przechowywania tokenów przypisany przez dostawców logowania</span><span class="sxs-lookup"><span data-stu-id="7b73e-119">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="7b73e-120">Przypisz dostawców społecznościowych logowania **identyfikator aplikacji** i **klucz tajny aplikacji** tokenów podczas procesu rejestracji.</span><span class="sxs-lookup"><span data-stu-id="7b73e-120">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="7b73e-121">Dokładne nazwy tokenu zależą od dostawcy.</span><span class="sxs-lookup"><span data-stu-id="7b73e-121">The exact token names vary by provider.</span></span> <span data-ttu-id="7b73e-122">Tokeny te reprezentują poświadczenia aplikacja korzysta z dostępu do swojego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="7b73e-122">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="7b73e-123">Tokeny stanowią "wpisy tajne", które mogą być połączone z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="7b73e-123">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="7b73e-124">Menedżer klucz tajny jest alternatywą bardziej bezpieczne przechowywanie tokenów w pliku konfiguracji, takich jak *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7b73e-124">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7b73e-125">Menedżer klucz tajny jest tylko do celów programowania.</span><span class="sxs-lookup"><span data-stu-id="7b73e-125">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="7b73e-126">Możesz przechowywać i chronić Azure testowych i produkcyjnych wpisów tajnych za pomocą [dostawcę konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="7b73e-126">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="7b73e-127">Postępuj zgodnie z instrukcjami w [bezpieczne przechowywanie kluczy tajnych aplikacji w trakcie opracowywania w programie ASP.NET Core](xref:security/app-secrets) tematu do przechowywania tokenów przypisany przez każdego dostawcy logowania poniżej.</span><span class="sxs-lookup"><span data-stu-id="7b73e-127">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="7b73e-128">Konfigurowanie dostawców logowania, wymagane przez aplikację</span><span class="sxs-lookup"><span data-stu-id="7b73e-128">Setup login providers required by your application</span></span>

<span data-ttu-id="7b73e-129">Użyj poniższych tematów, aby skonfigurować aplikację do używania odpowiednich dostawców:</span><span class="sxs-lookup"><span data-stu-id="7b73e-129">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="7b73e-130">[Facebook](xref:security/authentication/facebook-logins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="7b73e-130">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="7b73e-131">[W usłudze Twitter](xref:security/authentication/twitter-logins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="7b73e-131">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="7b73e-132">[Google](xref:security/authentication/google-logins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="7b73e-132">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="7b73e-133">[Microsoft](xref:security/authentication/microsoft-logins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="7b73e-133">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="7b73e-134">[Inni dostawcy](xref:security/authentication/otherlogins) instrukcje</span><span class="sxs-lookup"><span data-stu-id="7b73e-134">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="7b73e-135">Opcjonalnie Ustaw hasło</span><span class="sxs-lookup"><span data-stu-id="7b73e-135">Optionally set password</span></span>

<span data-ttu-id="7b73e-136">Po zarejestrowaniu się przy użyciu dostawcy logowania zewnętrznego, nie ma hasła zarejestrowane z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="7b73e-136">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="7b73e-137">Pozwala to uniknąć z tworzenia i zapamiętywanie hasła dla tej witryny, ale zapewnia także możesz zależne od dostawcy logowania zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="7b73e-137">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="7b73e-138">Jeśli dostawcy logowania zewnętrznego jest niedostępny, nie można zalogować się do witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="7b73e-138">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="7b73e-139">Aby utworzyć hasło i zalogować się przy użyciu ustawioną podczas procesu logowania z zewnętrznych dostawców poczty e-mail:</span><span class="sxs-lookup"><span data-stu-id="7b73e-139">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="7b73e-140">Wybierz **Hello &lt;aliasu adresu e-mail&gt;**  link w prawym górnym rogu, aby przejść do **Zarządzaj** widoku.</span><span class="sxs-lookup"><span data-stu-id="7b73e-140">Select the **Hello &lt;email alias&gt;** link at the top right corner to navigate to the **Manage** view.</span></span>

![Widok zarządzania aplikacji sieci Web](index/_static/pass1a.png)

* <span data-ttu-id="7b73e-142">Wybierz pozycję **Utwórz**</span><span class="sxs-lookup"><span data-stu-id="7b73e-142">Select **Create**</span></span>

![Ustawianie strony hasła](index/_static/pass2a.png)

* <span data-ttu-id="7b73e-144">Ustawione prawidłowe hasło i umożliwia to logowanie za pomocą poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="7b73e-144">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7b73e-145">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="7b73e-145">Next steps</span></span>

* <span data-ttu-id="7b73e-146">W tym artykule wprowadzono uwierzytelniania zewnętrznego i opisano wymagania wstępne dotyczące dodawania logowań zewnętrznych do aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7b73e-146">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="7b73e-147">Odwołanie do strony właściwe dla dostawcy Konfigurowanie logowania dla dostawcy, wymagane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="7b73e-147">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="7b73e-148">Można utrwalić dodatkowe dane dotyczące użytkownika i tokenami dostępu i odświeżania.</span><span class="sxs-lookup"><span data-stu-id="7b73e-148">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="7b73e-149">Aby uzyskać więcej informacji, zobacz <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="7b73e-149">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
