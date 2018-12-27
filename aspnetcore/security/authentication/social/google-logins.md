---
title: Ustawienia logowania zewnętrznego Google w programie ASP.NET Core
author: rick-anderson
description: Ten samouczek przedstawia integracja uwierzytelniania użytkownika konta Google do istniejącej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: 4bb5a36cf654deb694d60da126fa42baf382f729
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735768"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="de311-103">Ustawienia logowania zewnętrznego Google w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="de311-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="de311-104">Przez [Valeriy Novytskyy](https://github.com/01binary) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="de311-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="de311-105">W tym samouczku dowiesz się, jak umożliwić użytkownikom logowanie się przy użyciu swojego konta Google + przy użyciu przykładowy projekt programu ASP.NET Core 2.0, utworzony na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="de311-105">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="de311-106">Rozpoczniemy pracę, postępując zgodnie z [oficjalnych kroków](https://developers.google.com/identity/sign-in/web/devconsole-project) do utworzenia nowej aplikacji w konsoli interfejsu API Google.</span><span class="sxs-lookup"><span data-stu-id="de311-106">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="de311-107">Tworzenie aplikacji w konsoli interfejsu API Google</span><span class="sxs-lookup"><span data-stu-id="de311-107">Create the app in Google API Console</span></span>

* <span data-ttu-id="de311-108">Przejdź do [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) i zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="de311-108">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="de311-109">Jeśli nie masz już konto Google, użyj **więcej opcji** > **[Utwórz konto](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  łącze, aby go utworzyć:</span><span class="sxs-lookup"><span data-stu-id="de311-109">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Konsola interfejsu API Google](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="de311-111">Nastąpi przekierowanie do **Menedżer interfejsu API biblioteki** strony:</span><span class="sxs-lookup"><span data-stu-id="de311-111">You are redirected to **API Manager Library** page:</span></span>

![Docelowa na stronie biblioteki Menedżer interfejsu API](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="de311-113">Naciśnij pozycję **Utwórz** i wprowadź swoje **Nazwa projektu**:</span><span class="sxs-lookup"><span data-stu-id="de311-113">Tap **Create** and enter your **Project name**:</span></span>

![Okno dialogowe nowego projektu](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="de311-115">Po zaakceptowaniu okna dialogowego, nastąpi przekierowanie do strony biblioteki umożliwia wybranie funkcji dla nowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="de311-115">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="de311-116">Znajdź **interfejsu API Google +** na liście i kliknij jego łącze, aby dodać funkcję interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="de311-116">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![Wyszukaj "interfejs API Google +" na stronie biblioteki Menedżer interfejsu API](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="de311-118">Zostanie wyświetlona strona dla nowo dodanego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="de311-118">The page for the newly added API is displayed.</span></span> <span data-ttu-id="de311-119">Naciśnij pozycję **Włącz** dodać Google + znak w funkcji do aplikacji:</span><span class="sxs-lookup"><span data-stu-id="de311-119">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![Docelowa na stronie Menedżer interfejsu API Google + interfejs API](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="de311-121">Po włączeniu interfejsu API, naciśnij przycisk **Utwórz poświadczenia** skonfigurować wpisy tajne:</span><span class="sxs-lookup"><span data-stu-id="de311-121">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![Tworzenie przycisku poświadczenia na stronie Menedżer interfejsu API Google + interfejsu API](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="de311-123">Wybierz:</span><span class="sxs-lookup"><span data-stu-id="de311-123">Choose:</span></span>
  * <span data-ttu-id="de311-124">**Google+ API**</span><span class="sxs-lookup"><span data-stu-id="de311-124">**Google+ API**</span></span>
  * <span data-ttu-id="de311-125">**Serwer sieci Web (np. środowisko node.js, Tomcat)**, i</span><span class="sxs-lookup"><span data-stu-id="de311-125">**Web server (e.g. node.js, Tomcat)**, and</span></span>
  * <span data-ttu-id="de311-126">**Dane użytkownika**:</span><span class="sxs-lookup"><span data-stu-id="de311-126">**User data**:</span></span>

![Strona poświadczeń Menedżer interfejsu API: Dowiedz się, jaki rodzaj poświadczeń należy panelu](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="de311-128">Naciśnij pozycję **jakie poświadczenia są potrzebne?** której przejście do drugiego kroku konfiguracji aplikacji **Utwórz identyfikator klienta OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="de311-128">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![Strona poświadczeń Menedżer interfejsu API: Utwórz identyfikator klienta OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="de311-130">Ponieważ tworzymy projektu Google + przy użyciu tylko jednej funkcji (Zaloguj), możemy wprowadzić ten sam **nazwa** dla Identyfikatora klienta OAuth 2.0, użyliśmy dla projektu.</span><span class="sxs-lookup"><span data-stu-id="de311-130">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="de311-131">Wprowadź identyfikator URI programistycznych dzięki `/signin-google` dołączany do **identyfikatory URI przekierowania autoryzowanych** pola (na przykład: `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="de311-131">Enter your development URI with `/signin-google` appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="de311-132">Uwierzytelnianie serwisu Google skonfigurowane w dalszej części tego samouczka będzie automatycznie obsługiwać żądań w `/signin-google` trasy do zaimplementowania przepływu uwierzytelniania OAuth.</span><span class="sxs-lookup"><span data-stu-id="de311-132">The Google authentication configured later in this tutorial will automatically handle requests at `/signin-google` route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="de311-133">Segmentem identyfikatora URI `/signin-google` jest ustawiony jako zwrotnym domyślnego dostawcę uwierzytelniania serwisu Google.</span><span class="sxs-lookup"><span data-stu-id="de311-133">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="de311-134">Identyfikator URI wywołania zwrotnego domyślne można zmienić podczas konfigurowania oprogramowania pośredniczącego uwierzytelniania Google za pośrednictwem dziedziczonego [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) właściwość [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) klasy.</span><span class="sxs-lookup"><span data-stu-id="de311-134">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

* <span data-ttu-id="de311-135">Naciśnij klawisz TAB, aby dodać **identyfikatory URI przekierowania autoryzowanych** wpisu.</span><span class="sxs-lookup"><span data-stu-id="de311-135">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="de311-136">Naciśnij pozycję **Utwórz identyfikator klienta**, która umożliwia przejście do trzeci krok **skonfigurować ekran zgody OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="de311-136">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![Strona poświadczeń Menedżer interfejsu API: Ustawianie ekranu wyrażania zgody OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="de311-138">Wprowadź swoje użytek publiczny **adres E-mail** i **nazwa produktu** wyświetlany dla aplikacji, gdy Google + monituje użytkownika do logowania.</span><span class="sxs-lookup"><span data-stu-id="de311-138">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="de311-139">Dodatkowe opcje są dostępne w obszarze **dodatkowe opcje dostosowywania**.</span><span class="sxs-lookup"><span data-stu-id="de311-139">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="de311-140">Naciśnij pozycję **Kontynuuj** do przejdź do ostatniego kroku **Pobierz poświadczenia**:</span><span class="sxs-lookup"><span data-stu-id="de311-140">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![Strona poświadczeń Menedżer interfejsu API: Pobierz poświadczenia](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="de311-142">Naciśnij pozycję **Pobierz** można zapisać plik w formacie JSON za pomocą kluczy tajnych aplikacji, a **gotowe** aby ukończyć tworzenie nowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="de311-142">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="de311-143">W przypadku wdrażania w witrynie będzie konieczne ponownie przeanalizowanie **konsoli Google** i rejestrowanie nowego publicznego adresu url.</span><span class="sxs-lookup"><span data-stu-id="de311-143">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="de311-144">Store Google ClientID i ClientSecret</span><span class="sxs-lookup"><span data-stu-id="de311-144">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="de311-145">Link ustawień poufnych, takich jak Google `Client ID` i `Client Secret` z konfiguracji aplikacji za pomocą [Menedżera klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="de311-145">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="de311-146">Do celów tego samouczka, nazwa tokeny `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="de311-146">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="de311-147">Wartości dla tych tokenów można znaleźć w pliku JSON, pobrany w poprzednim kroku, w obszarze `web.client_id` i `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="de311-147">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="de311-148">Konfigurowanie uwierzytelniania serwisu Google</span><span class="sxs-lookup"><span data-stu-id="de311-148">Configure Google Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="de311-149">Dodaj usługę Google w `ConfigureServices` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="de311-149">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="de311-150">Szablon projektu, w tym samouczku używane zapewnia, że [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) pakiet jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="de311-150">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

* <span data-ttu-id="de311-151">Aby zainstalować ten pakiet za pomocą programu Visual Studio 2017, kliknij prawym przyciskiem myszy projekt i wybierz pozycję **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="de311-151">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="de311-152">Aby zainstalować przy użyciu interfejsu wiersza polecenia platformy .NET Core, wykonaj następujące czynności w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="de311-152">To install with .NET Core CLI, execute the following in your project directory:</span></span>

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

<span data-ttu-id="de311-153">Dodaj oprogramowanie pośredniczące Google w `Configure` method in Class metoda *Startup.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="de311-153">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

<span data-ttu-id="de311-154">Zobacz [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) dokumentacja interfejsu API, aby uzyskać więcej informacji na temat opcji konfiguracji obsługiwanych przez uwierzytelnianie serwisu Google.</span><span class="sxs-lookup"><span data-stu-id="de311-154">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="de311-155">Może to służyć do żądania różne informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="de311-155">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="de311-156">Zaloguj się przy użyciu Google</span><span class="sxs-lookup"><span data-stu-id="de311-156">Sign in with Google</span></span>

<span data-ttu-id="de311-157">Uruchom aplikację, a następnie kliknij przycisk **Zaloguj**.</span><span class="sxs-lookup"><span data-stu-id="de311-157">Run your application and click **Log in**.</span></span> <span data-ttu-id="de311-158">Zostanie wyświetlona opcja Zaloguj się przy użyciu Google:</span><span class="sxs-lookup"><span data-stu-id="de311-158">An option to sign in with Google appears:</span></span>

![Aplikacja sieci Web jest uruchomiony w programie Microsoft Edge: Użytkownik nie jest uwierzytelniony](index/_static/DoneGoogle.png)

<span data-ttu-id="de311-160">Po kliknięciu w usłudze Google, nastąpi przekierowanie do usługi Google do uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="de311-160">When you click on Google, you are redirected to Google for authentication:</span></span>

![Okno dialogowe uwierzytelniania Google](index/_static/GoogleLogin.png)

<span data-ttu-id="de311-162">Po wprowadzeniu poświadczeń Google, następnie nastąpi przekierowanie do witryny sieci web, w którym można ustawić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="de311-162">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="de311-163">Obecnie zalogowano Cię przy użyciu poświadczeń konta Google:</span><span class="sxs-lookup"><span data-stu-id="de311-163">You are now logged in using your Google credentials:</span></span>

![Aplikacja sieci Web jest uruchomiony w programie Microsoft Edge: Użytkownik uwierzytelniony](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="de311-165">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="de311-165">Troubleshooting</span></span>

* <span data-ttu-id="de311-166">Jeśli zostanie wyświetlony `403 (Forbidden)` stronę błędu z własnej aplikacji, gdy działa w trybie projektowania (lub przerwanie w debugerze z tym samym błędem), upewnij się, że **interfejsu API Google +** zostało włączone w **Menedżer interfejsu API biblioteki** przez wykonanie kroków podanych [wcześniej na tej stronie](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="de311-166">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="de311-167">Jeśli nie otrzymujesz błędy logowania nie rozwiąże problemu, przełącz się do trybu opracowywania, aby ułatwić debugowanie problemu.</span><span class="sxs-lookup"><span data-stu-id="de311-167">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="de311-168">**ASP.NET Core 2.x tylko:** Jeśli tożsamość nie jest skonfigurowana, wywołując `services.AddIdentity` w `ConfigureServices`, próby uwierzytelnienia będą powodować *ArgumentException: Opcja "SignInScheme" musi być podana*.</span><span class="sxs-lookup"><span data-stu-id="de311-168">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="de311-169">Szablon projektu, w tym samouczku używane gwarantuje, że odbywa się.</span><span class="sxs-lookup"><span data-stu-id="de311-169">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="de311-170">Jeśli nie utworzono bazy danych lokacji, stosując początkowej migracji, zostanie wyświetlony *operacji bazy danych nie powiodło się podczas przetwarzania żądania* błędu.</span><span class="sxs-lookup"><span data-stu-id="de311-170">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="de311-171">Naciśnij pozycję **zastosować migracje** do tworzenia bazy danych i Odśwież, aby kontynuować po błędzie.</span><span class="sxs-lookup"><span data-stu-id="de311-171">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="de311-172">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="de311-172">Next steps</span></span>

* <span data-ttu-id="de311-173">W tym artykule pokazano, jak można uwierzytelniać za pomocą usługi Google.</span><span class="sxs-lookup"><span data-stu-id="de311-173">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="de311-174">Możesz wykonać podejście podobne do uwierzytelniania przy użyciu innych dostawców na [poprzedniej strony](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="de311-174">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="de311-175">Gdy publikujesz witrynę sieci web do aplikacji sieci web platformy Azure, należy zresetować `ClientSecret` w konsoli interfejsu API Google.</span><span class="sxs-lookup"><span data-stu-id="de311-175">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="de311-176">Ustaw `Authentication:Google:ClientId` i `Authentication:Google:ClientSecret` jako ustawienia aplikacji w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="de311-176">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="de311-177">System konfiguracji jest skonfigurowany do odczytu klucze ze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="de311-177">The configuration system is set up to read keys from environment variables.</span></span>
