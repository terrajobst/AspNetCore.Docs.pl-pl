---
title: "Facebook, Google i zewnętrznego dostawcy uwierzytelniania w ASP.NET Core"
author: rick-anderson
description: "W tym samouczku pokazano, jak kompilacji platformy ASP.NET Core 2.x aplikacji przy użyciu protokołu OAuth 2.0 przy pomocy dostawcy uwierzytelniania zewnętrznego."
manager: wpickett
ms.author: riande
ms.date: 11/01/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/social/index
ms.openlocfilehash: 76433f814d6850a449434c29eb0bd27570ce193a
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="aca88-103">Facebook, Google i zewnętrznego dostawcy uwierzytelniania w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aca88-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<a name="security-authentication-social-logins"></a>

<span data-ttu-id="aca88-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aca88-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aca88-105">W tym samouczku przedstawiono sposób kompilacji platformy ASP.NET Core 2.x aplikacji, która umożliwia użytkownikom zalogowanie się przy użyciu protokołu OAuth 2.0 z poświadczeń zewnętrzni dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="aca88-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="aca88-106">[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), i [Microsoft](microsoft-logins.md) dostawców są przedstawione w poniższych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="aca88-106">[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), and [Microsoft](microsoft-logins.md) providers are covered in the following sections.</span></span> <span data-ttu-id="aca88-107">Inni dostawcy są dostępne w pakiety innych firm takich jak [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) i [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="aca88-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Ikony mediów społecznościowych dla usługi Facebook, Twitter, Google plus i systemu Windows](index/_static/social.png)

<span data-ttu-id="aca88-109">Umożliwienie użytkownikom logowania się przy użyciu swoich istniejących poświadczeń jest wygodne dla użytkowników i przenosi wiele złożoności Zarządzanie procesem logowania na innych firm.</span><span class="sxs-lookup"><span data-stu-id="aca88-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="aca88-110">Przykłady jak społecznościowych logowania można dysku konwersje typów ruchu i klienta, można znaleźć przypadków przez [Facebook](https://www.facebook.com/unsupportedbrowser) i [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="aca88-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="aca88-111">Uwaga: Przedstawionych w tym miejscu pakiety abstrakcyjnej dużą złożoność przepływ uwierzytelniania OAuth, ale opis szczegóły mogą okazać się konieczne podczas rozwiązywania problemów.</span><span class="sxs-lookup"><span data-stu-id="aca88-111">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="aca88-112">Wiele zasobów są dostępne; na przykład, zobacz [wprowadzenie do OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) lub [2 OAuth opis](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span><span class="sxs-lookup"><span data-stu-id="aca88-112">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="aca88-113">Niektóre problemy można rozwiązać, analizując [kod źródłowy platformy ASP.NET Core dla pakietów dostawcy](https://github.com/aspnet/Security/tree/dev/src).</span><span class="sxs-lookup"><span data-stu-id="aca88-113">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="aca88-114">Utwórz nowy projekt platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aca88-114">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="aca88-115">W programie Visual Studio 2017 r, Utwórz nowy projekt z stronę początkową lub za pośrednictwem **Plik > Nowy > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="aca88-115">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="aca88-116">Wybierz **aplikacji sieci Web platformy ASP.NET Core** dostępne w szablonie **Visual C# > .NET Core** kategorii:</span><span class="sxs-lookup"><span data-stu-id="aca88-116">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![Okno dialogowe nowego projektu](index/_static/new-project.png)

* <span data-ttu-id="aca88-118">Wybierz **aplikacji sieci Web** i sprawdź **uwierzytelniania** ustawiono **indywidualnych kont użytkowników**:</span><span class="sxs-lookup"><span data-stu-id="aca88-118">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Okno dialogowe nowego aplikacji sieci Web](index/_static/select-project.png)

<span data-ttu-id="aca88-120">Uwaga: Ten samouczek dotyczy wersji platformy ASP.NET Core SDK 2.0, który można wybrać w górnej części kreatora.</span><span class="sxs-lookup"><span data-stu-id="aca88-120">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="aca88-121">Zastosuj migracje</span><span class="sxs-lookup"><span data-stu-id="aca88-121">Apply migrations</span></span>

* <span data-ttu-id="aca88-122">Uruchom aplikację i wybierz **Zaloguj** łącza.</span><span class="sxs-lookup"><span data-stu-id="aca88-122">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="aca88-123">Wybierz **Zarejestruj się jako nowy użytkownik** łącza.</span><span class="sxs-lookup"><span data-stu-id="aca88-123">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="aca88-124">Wprowadź adres e-mail i hasło dla nowego konta, a następnie wybierz **zarejestrować**.</span><span class="sxs-lookup"><span data-stu-id="aca88-124">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="aca88-125">Postępuj zgodnie z instrukcjami, aby zastosować migracji.</span><span class="sxs-lookup"><span data-stu-id="aca88-125">Follow the instructions to apply migrations.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="aca88-126">Wymagaj protokołu SSL</span><span class="sxs-lookup"><span data-stu-id="aca88-126">Require SSL</span></span>

<span data-ttu-id="aca88-127">OAuth 2.0 wymaga korzystania z protokołu SSL do uwierzytelniania za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="aca88-127">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="aca88-128">Uwaga: Projekty utworzone za pomocą **aplikacji sieci Web** lub **interfejsu API sieci Web** szablonów projektu dla platformy ASP.NET Core 2.x są automatycznie konfigurowane do włączenia protokołu SSL, a następnie uruchom z adresem URL https, jeśli **poszczególnych Konta użytkowników** została zaznaczona opcja **okno dialogowe Zmienianie uwierzytelniania** w Kreatorze projektu, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="aca88-128">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="aca88-129">Wymagaj protokołu SSL w witrynie, wykonując kroki opisane w [Wymuszanie protokołu SSL w aplikacji platformy ASP.NET Core](xref:security/enforcing-ssl) tematu.</span><span class="sxs-lookup"><span data-stu-id="aca88-129">Require SSL on your site by following the steps in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="aca88-130">SecretManager używany do przechowywania tokenów przypisany przez dostawców logowania</span><span class="sxs-lookup"><span data-stu-id="aca88-130">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="aca88-131">Przypisz dostawców logowania społecznościowych **identyfikator aplikacji** i **klucz tajny aplikacji** tokeny podczas procesu rejestracji (dokładne nazewnictwa jest zależna od dostawcy).</span><span class="sxs-lookup"><span data-stu-id="aca88-131">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process (exact naming varies by provider).</span></span>

<span data-ttu-id="aca88-132">Te wartości są wydajnie *nazwy użytkownika* i *hasło* aplikacja korzysta z dostępu do interfejsu API i stanowią "kluczy tajnych" połączone za pomocą konfiguracji aplikacji **Manager klucz tajny** zamiast przechowywania ich w plikach konfiguracji bezpośrednio lub kodować je.</span><span class="sxs-lookup"><span data-stu-id="aca88-132">These values are effectively the *user name* and *password* your application uses to access their API, and constitute the "secrets" that can be linked to your application configuration with the help of **Secret Manager** instead of storing them in configuration files directly or hard-coding them.</span></span>

<span data-ttu-id="aca88-133">Postępuj zgodnie z instrukcjami [bezpiecznego magazynu kluczy tajnych aplikacji w czasie opracowywania w ASP.NET Core](xref:security/app-secrets) tematu, dzięki czemu można przechowywać tokeny przypisany przez każdy dostawca logowania poniżej.</span><span class="sxs-lookup"><span data-stu-id="aca88-133">Follow the steps in [Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets) topic so that you can store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="aca88-134">Dostawców logowania Instalatora wymagane przez aplikację</span><span class="sxs-lookup"><span data-stu-id="aca88-134">Setup login providers required by your application</span></span>

<span data-ttu-id="aca88-135">Aby skonfigurować aplikację do używania odpowiednich dostawców, należy użyć następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="aca88-135">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="aca88-136">[Facebook](facebook-logins.md) instrukcje</span><span class="sxs-lookup"><span data-stu-id="aca88-136">[Facebook](facebook-logins.md) instructions</span></span>
* <span data-ttu-id="aca88-137">[W usłudze Twitter](twitter-logins.md) instrukcje</span><span class="sxs-lookup"><span data-stu-id="aca88-137">[Twitter](twitter-logins.md) instructions</span></span>
* <span data-ttu-id="aca88-138">[Google](google-logins.md) instrukcje</span><span class="sxs-lookup"><span data-stu-id="aca88-138">[Google](google-logins.md) instructions</span></span>
* <span data-ttu-id="aca88-139">[Microsoft](microsoft-logins.md) instrukcje</span><span class="sxs-lookup"><span data-stu-id="aca88-139">[Microsoft](microsoft-logins.md) instructions</span></span>
* <span data-ttu-id="aca88-140">[Inny dostawca](other-logins.md) instrukcje</span><span class="sxs-lookup"><span data-stu-id="aca88-140">[Other provider](other-logins.md) instructions</span></span>

## <a name="optionally-set-password"></a><span data-ttu-id="aca88-141">Opcjonalnie Ustaw hasło.</span><span class="sxs-lookup"><span data-stu-id="aca88-141">Optionally set password</span></span>

<span data-ttu-id="aca88-142">Podczas rejestrowania przy użyciu dostawcy logowania zewnętrznego nie ma hasła w zarejestrowany z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="aca88-142">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="aca88-143">Pozwala to uniknąć z tworzeniem i zapamiętywanie hasła dla lokacji, ale zapewnia także możesz zależny od dostawcy logowania zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="aca88-143">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="aca88-144">Jeśli dostawcy logowania zewnętrznego jest niedostępny, nie będzie można logować się do witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="aca88-144">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="aca88-145">Aby utworzyć hasło i zaloguj się przy użyciu poczty e-mail ustawioną podczas logowania w procesie z zewnętrznych źródeł:</span><span class="sxs-lookup"><span data-stu-id="aca88-145">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="aca88-146">Wybierz **Hello <email alias>**  łącze w prawym górnym rogu, aby przejść do **Zarządzaj** widoku.</span><span class="sxs-lookup"><span data-stu-id="aca88-146">Tap the **Hello <email alias>** link at the top right corner to navigate to the **Manage** view.</span></span>

![Widok Zarządzaj aplikacji sieci Web](index/_static/pass1a.png)

* <span data-ttu-id="aca88-148">Wybierz **tworzenie**</span><span class="sxs-lookup"><span data-stu-id="aca88-148">Tap **Create**</span></span>

![Ustawianie hasła strony](index/_static/pass2a.png)

* <span data-ttu-id="aca88-150">Ustawione prawidłowe hasło i umożliwia to Zaloguj się przy użyciu poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="aca88-150">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aca88-151">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="aca88-151">Next steps</span></span>

* <span data-ttu-id="aca88-152">W tym artykule wprowadzono uwierzytelniania zewnętrznego i wyjaśniono wymagania wstępne dotyczące dodawania logowań zewnętrznych do aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aca88-152">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="aca88-153">Odwołanie do strony specyficznego dla dostawcy można skonfigurować logowania dla dostawcy wymagane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="aca88-153">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
