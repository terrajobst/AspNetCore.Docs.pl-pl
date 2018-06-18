---
title: Ustawienia logowania zewnętrznego Google w ASP.NET Core
author: rick-anderson
description: W tym samouczku przedstawiono integrację uwierzytelnianie użytkownika konto Google do istniejącej aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/google-logins
ms.openlocfilehash: 878c0b16e24f48a0ee84f93393af67af1728e284
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2018
ms.locfileid: "35725968"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="75af7-103">Ustawienia logowania zewnętrznego Google w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="75af7-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="75af7-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="75af7-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="75af7-105">W tym samouczku przedstawiono sposób umożliwić użytkownikom logowanie za pomocą swojego konta Google + przy użyciu przykładowy projekt platformy ASP.NET Core 2.0, utworzony na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="75af7-105">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="75af7-106">Rozpoczniemy wykonując [kroki oficjalnego](https://developers.google.com/identity/sign-in/web/devconsole-project) do utworzenia nowej aplikacji w konsoli interfejsu API firmy Google.</span><span class="sxs-lookup"><span data-stu-id="75af7-106">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="75af7-107">Tworzenie aplikacji w konsoli interfejsu API firmy Google</span><span class="sxs-lookup"><span data-stu-id="75af7-107">Create the app in Google API Console</span></span>

* <span data-ttu-id="75af7-108">Przejdź do [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) i zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="75af7-108">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="75af7-109">Jeśli nie masz już konto Google, użyj **więcej opcji** > **[Tworzenie konta](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  łącze, aby go utworzyć:</span><span class="sxs-lookup"><span data-stu-id="75af7-109">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Konsoli interfejsu API firmy Google](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="75af7-111">Nastąpi przekierowanie do **biblioteki interfejsu API menedżera** strony:</span><span class="sxs-lookup"><span data-stu-id="75af7-111">You are redirected to **API Manager Library** page:</span></span>

![Strona Menedżer interfejsu API biblioteki](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="75af7-113">Wybierz **Utwórz** , a następnie wprowadź Twojej **Nazwa projektu**:</span><span class="sxs-lookup"><span data-stu-id="75af7-113">Tap **Create** and enter your **Project name**:</span></span>

![Okno dialogowe nowego projektu](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="75af7-115">Po zaakceptowaniu okno dialogowe, są przekierowany do strony biblioteki, dzięki czemu można wybrać funkcje dla nowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="75af7-115">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="75af7-116">Znajdź **interfejsu API Google +** na liście i kliknij jego łącze, aby dodać funkcję interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="75af7-116">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![Strona Menedżer interfejsu API biblioteki](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="75af7-118">Zostanie wyświetlona strona dla nowo dodanego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="75af7-118">The page for the newly added API is displayed.</span></span> <span data-ttu-id="75af7-119">Wybierz **włączyć** dodać Google + znak w funkcji do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="75af7-119">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![Strona Menedżer interfejsu API Google + interfejsu API](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="75af7-121">Po włączeniu interfejs API, naciśnij przycisk **Utwórz poświadczenia** skonfigurować kluczy tajnych:</span><span class="sxs-lookup"><span data-stu-id="75af7-121">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![Strona Menedżer interfejsu API Google + interfejsu API](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="75af7-123">Wybierz:</span><span class="sxs-lookup"><span data-stu-id="75af7-123">Choose:</span></span>
   * <span data-ttu-id="75af7-124">**Google+ API**</span><span class="sxs-lookup"><span data-stu-id="75af7-124">**Google+ API**</span></span>
   * <span data-ttu-id="75af7-125">**Serwer sieci Web (np. node.js, Tomcat)**, i</span><span class="sxs-lookup"><span data-stu-id="75af7-125">**Web server (e.g. node.js, Tomcat)**, and</span></span>
   * <span data-ttu-id="75af7-126">**Dane użytkownika**:</span><span class="sxs-lookup"><span data-stu-id="75af7-126">**User data**:</span></span>

![Strona poświadczeń Menedżer interfejsu API: Sprawdź, jaki rodzaj poświadczeń można muszą panelu](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="75af7-128">Wybierz **poświadczeniami, które są potrzebne?** który przyjmuje do drugiego kroku konfiguracji aplikacji **Utwórz identyfikator klienta OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="75af7-128">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![Strona poświadczeń Menedżer interfejsu API: Utwórz identyfikator klienta OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="75af7-130">Ponieważ tworzymy projektu Google + z tylko jedną funkcję (logowania), możemy wprowadzić ten sam **nazwa** dla Identyfikatora klienta OAuth 2.0, użyliśmy dla projektu.</span><span class="sxs-lookup"><span data-stu-id="75af7-130">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="75af7-131">Wprowadź identyfikator URI programowania z `/signin-google` dołączany do **autoryzowanych przekierowania URI** pola (na przykład: `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="75af7-131">Enter your development URI with `/signin-google` appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="75af7-132">Uwierzytelnianie serwisu Google skonfigurowane w dalszej części tego samouczka automatycznie będzie obsługiwać żądań w `/signin-google` trasy do zaimplementowania przepływu OAuth.</span><span class="sxs-lookup"><span data-stu-id="75af7-132">The Google authentication configured later in this tutorial will automatically handle requests at `/signin-google` route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="75af7-133">Segment identyfikatora URI `/signin-google` jest ustawiony jako domyślny wywołania zwrotnego z dostawcę uwierzytelniania serwisu Google.</span><span class="sxs-lookup"><span data-stu-id="75af7-133">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="75af7-134">Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania Google za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) klasy.</span><span class="sxs-lookup"><span data-stu-id="75af7-134">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

* <span data-ttu-id="75af7-135">Naciśnij klawisz TAB, aby dodać **autoryzowanych przekierowania URI** wpisu.</span><span class="sxs-lookup"><span data-stu-id="75af7-135">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="75af7-136">Wybierz **Utwórz identyfikator klienta**, który umożliwia przejście do trzeciego stopnia **ustawienie ekranu zgoda OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="75af7-136">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![Strona poświadczeń Menedżer interfejsu API: ustawianie ekranu zgoda OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="75af7-138">Wprowadź skierowane do publicznego **adres E-mail** i **nazwa produktu** wyświetlany dla aplikacji, jeśli Google + monity użytkownika do logowania.</span><span class="sxs-lookup"><span data-stu-id="75af7-138">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="75af7-139">Dodatkowe opcje są dostępne w obszarze **więcej opcji dostosowania**.</span><span class="sxs-lookup"><span data-stu-id="75af7-139">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="75af7-140">Wybierz **Kontynuuj** do przejdź do ostatniego kroku **Pobierz poświadczenia**:</span><span class="sxs-lookup"><span data-stu-id="75af7-140">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![Strona poświadczeń Menedżer interfejsu API: Pobierz poświadczenia](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="75af7-142">Wybierz **Pobierz** można zapisać pliku JSON z haseł aplikacji i **gotowe** aby zakończyć tworzenie nowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="75af7-142">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="75af7-143">W przypadku wdrażania lokacji należy ponownie **konsoli Google** i zarejestrować nowego publicznego adresu url.</span><span class="sxs-lookup"><span data-stu-id="75af7-143">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="75af7-144">Sklep Google ClientID i ClientSecret</span><span class="sxs-lookup"><span data-stu-id="75af7-144">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="75af7-145">Link ustawień poufnych, takich jak Google `Client ID` i `Client Secret` do swojej aplikacji konfiguracji za pomocą [Manager klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="75af7-145">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="75af7-146">Do celów tego samouczka, nazwa tokeny `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="75af7-146">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="75af7-147">Tokeny te wartości można znaleźć w pliku JSON pobranego w poprzednim kroku, w obszarze `web.client_id` i `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="75af7-147">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="75af7-148">Konfigurowanie uwierzytelniania serwisu Google</span><span class="sxs-lookup"><span data-stu-id="75af7-148">Configure Google Authentication</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="75af7-149">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="75af7-149">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="75af7-150">Dodaj usługę Google w `ConfigureServices` metody w *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="75af7-150">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](~/includes/chain-auth-providers.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="75af7-151">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="75af7-151">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="75af7-152">Szablon projektu używany w tym samouczku upewnia się, że [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) pakiet jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="75af7-152">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

* <span data-ttu-id="75af7-153">Aby zainstalować ten pakiet przy użyciu programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="75af7-153">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="75af7-154">Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="75af7-154">To install with .NET Core CLI, execute the following in your project directory:</span></span>

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

<span data-ttu-id="75af7-155">Dodaj oprogramowanie pośredniczące Google w `Configure` metody w *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="75af7-155">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

<span data-ttu-id="75af7-156">Zobacz [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelniania serwisu Google.</span><span class="sxs-lookup"><span data-stu-id="75af7-156">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="75af7-157">To może być używane do żądania różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="75af7-157">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="75af7-158">Zaloguj się przy użyciu usługi Google</span><span class="sxs-lookup"><span data-stu-id="75af7-158">Sign in with Google</span></span>

<span data-ttu-id="75af7-159">Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="75af7-159">Run your application and click **Log in**.</span></span> <span data-ttu-id="75af7-160">Pojawia się opcję, aby zalogować się przy użyciu usługi Google:</span><span class="sxs-lookup"><span data-stu-id="75af7-160">An option to sign in with Google appears:</span></span>

![Aplikacji sieci Web w programie Microsoft Edge: użytkownik nie jest uwierzytelniony](index/_static/DoneGoogle.png)

<span data-ttu-id="75af7-162">Po kliknięciu Google, nastąpi przekierowanie do sklepu Google uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="75af7-162">When you click on Google, you are redirected to Google for authentication:</span></span>

![Dialog uwierzytelniania Google](index/_static/GoogleLogin.png)

<span data-ttu-id="75af7-164">Po wprowadzeniu poświadczeń Google, następnie nastąpi przekierowanie do witryny sieci web, w którym można ustawić adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="75af7-164">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="75af7-165">Użytkownik jest obecnie zalogowany przy użyciu poświadczeń konta Google:</span><span class="sxs-lookup"><span data-stu-id="75af7-165">You are now logged in using your Google credentials:</span></span>

![Aplikacji sieci Web w programie Microsoft Edge: użytkownik uwierzytelniony](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="75af7-167">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="75af7-167">Troubleshooting</span></span>

* <span data-ttu-id="75af7-168">Jeśli zostanie wyświetlony `403 (Forbidden)` strony błędu z własną aplikację podczas pracy w trybie Programowanie (lub podziału do debugera z tego samego błędu), upewnij się, że **interfejsu API Google +** została włączona w **biblioteki interfejsu API menedżera** przez wykonanie kroków podanych [wcześniej na tej stronie](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="75af7-168">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="75af7-169">Jeśli logowanie nie działa, a nie ma dostępnych żadnych błędów, przełącz się do trybu programowanie ułatwiające debugować ten problem.</span><span class="sxs-lookup"><span data-stu-id="75af7-169">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="75af7-170">**Platformy ASP.NET Core tylko 2.x:** Jeśli tożsamości nie jest skonfigurowana pod wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: należy podać opcję "SignInScheme"*.</span><span class="sxs-lookup"><span data-stu-id="75af7-170">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="75af7-171">Szablon projektu używany w tym samouczku zapewnia to zrobić.</span><span class="sxs-lookup"><span data-stu-id="75af7-171">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="75af7-172">Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, otrzyma *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu.</span><span class="sxs-lookup"><span data-stu-id="75af7-172">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="75af7-173">Wybierz **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować mimo błędu.</span><span class="sxs-lookup"><span data-stu-id="75af7-173">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="75af7-174">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="75af7-174">Next steps</span></span>

* <span data-ttu-id="75af7-175">W tym artykule pokazano, jak można uwierzytelniać z serwisem Google.</span><span class="sxs-lookup"><span data-stu-id="75af7-175">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="75af7-176">Można wykonać podobne podejścia do uwierzytelniania za pomocą innych dostawców wymienione na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="75af7-176">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="75af7-177">Po opublikowaniu witryny sieci web do aplikacji sieci web platformy Azure, należy zresetować `ClientSecret` w konsoli interfejsu API firmy Google.</span><span class="sxs-lookup"><span data-stu-id="75af7-177">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="75af7-178">Ustaw `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret` zgodnie z ustawieniami aplikacji w portalu Azure.</span><span class="sxs-lookup"><span data-stu-id="75af7-178">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="75af7-179">System konfiguracji jest skonfigurowany do odczytu klucze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="75af7-179">The configuration system is set up to read keys from environment variables.</span></span>
