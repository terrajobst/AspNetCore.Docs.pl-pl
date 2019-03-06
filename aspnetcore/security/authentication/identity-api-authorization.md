---
title: Wprowadzenie do uwierzytelniania dla aplikacji programu ASP.NET Core
author: ''
description: Tożsamość za pomocą aplikacji jednostronicowej hostowaną wewnątrz aplikacji ASP.NET Core.
ms.author: ''
ms.date: 03/05/2018
uid: security/authentication/identity/spa
ms.openlocfilehash: cf04ec1ff0ae9afea066fd1864ab0a7956ace32c
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346821"
---
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="bbc66-103">Uwierzytelnianie i autoryzację dla aplikacji jednostronicowych</span><span class="sxs-lookup"><span data-stu-id="bbc66-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="bbc66-104">W programie ASP.NET 3.0 wprowadzamy obsługę uwierzytelniania w aplikacji jednostronicowej przy użyciu naszej obsługi nowego interfejsu API autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-104">In ASP.NET 3.0 we are introducing support for authentication in single page applications using our new support for API authorization.</span></span> <span data-ttu-id="bbc66-105">Ta funkcja opiera się na kombinacji tożsamości platformy ASP.NET Core uwierzytelniania i przechowywanie użytkowników i tożsamość serwera dotyczące implementowania Open ID Connect.</span><span class="sxs-lookup"><span data-stu-id="bbc66-105">This support is based on a combination of ASP.NET Core Identity for authenticating and storing users and Identity Server for implementing Open ID Connect.</span></span>

<span data-ttu-id="bbc66-106">Dodaliśmy nowy parametr uwierzytelniania do naszej platformy Angular i szablonów platformy React, które są podobne do parametru uwierzytelniania w naszych szablonów stron mvc i razor za pomocą dozwolone wartości "None" i "Indywidualne".</span><span class="sxs-lookup"><span data-stu-id="bbc66-106">We have added a new authentication parameter to our Angular and React templates that is similar to the authentication parameter in our mvc and razor pages templates with allowed values 'None' and 'Individual'.</span></span>

## <a name="create-an-angular-app-with-api-authorization-support"></a><span data-ttu-id="bbc66-107">Tworzenie aplikacji Angular dzięki obsłudze autoryzacji interfejsu API</span><span class="sxs-lookup"><span data-stu-id="bbc66-107">Create an Angular app with API authorization support</span></span>

<span data-ttu-id="bbc66-108">Aby utworzyć nową aplikację platformy Angular dzięki obsłudze uwierzytelniania i autoryzacji użytkowników, Otwórz powłokę wiersza polecenia i uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="bbc66-108">To create a new Angular app with support for authentication and authorization of users, open a command shell and run the following command:</span></span>

```console
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="bbc66-109">Poprzednie polecenie umożliwia utworzenie aplikacji platformy ASP.NET Core za pomocą *ClientApp* katalog zawierający aplikacji Angular.</span><span class="sxs-lookup"><span data-stu-id="bbc66-109">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the Angular app.</span></span>

## <a name="create-a-react-app-with-api-authorization-support"></a><span data-ttu-id="bbc66-110">Tworzenie aplikacji platformy React z obsługi autoryzacji interfejsu API</span><span class="sxs-lookup"><span data-stu-id="bbc66-110">Create a React app with API authorization support</span></span>

<span data-ttu-id="bbc66-111">Aby utworzyć nową aplikację platformy React z obsługą uwierzytelniania i autoryzacji użytkowników, Otwórz powłokę wiersza polecenia i uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="bbc66-111">To create a new React app with support for authentication and authorization of users, open a command shell and run the following command:</span></span>

```console
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="bbc66-112">Poprzednie polecenie umożliwia utworzenie aplikacji platformy ASP.NET Core za pomocą *ClientApp* katalogu zawierającego aplikację platformy React.</span><span class="sxs-lookup"><span data-stu-id="bbc66-112">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the React app.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="bbc66-113">Ogólny opis składników platformy ASP.NET Core w aplikacji</span><span class="sxs-lookup"><span data-stu-id="bbc66-113">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="bbc66-114">Istnieje kilka ma zostać dodany do projektu, gdy firma Microsoft zapewnia obsługę dla uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="bbc66-114">There are several additions to the project when we include support for authentication:</span></span>

### <a name="startup-class"></a><span data-ttu-id="bbc66-115">Klasa początkowa</span><span class="sxs-lookup"><span data-stu-id="bbc66-115">Startup class</span></span>

<span data-ttu-id="bbc66-116">Jeśli spojrzymy na kod w klasie uruchamiania poniżej Jesteśmy wdzięczni za dołączenia następujące:</span><span class="sxs-lookup"><span data-stu-id="bbc66-116">If we look at the code in the Startup class below we can appreciate the following inclusions:</span></span>
* <span data-ttu-id="bbc66-117">Wewnątrz `public void ConfigureServices(IServiceCollection services)`:</span><span class="sxs-lookup"><span data-stu-id="bbc66-117">Inside `public void ConfigureServices(IServiceCollection services)`:</span></span>
  * <span data-ttu-id="bbc66-118">Tożsamość przy użyciu domyślnego interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="bbc66-118">Identity with the default UI.</span></span>
    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
      options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
      .AddDefaultUI(UIFramework.Bootstrap4)
      .AddEntityFrameworkStores<ApplicationDbContext>();
    ```
  * <span data-ttu-id="bbc66-119">Tożsamość serwera przy użyciu dodatkowej metody pomocnika AddApiAuthorization tej konfiguracji niektóre domyślne konwencje platformy ASP.NET na podstawie tożsamości serwera.</span><span class="sxs-lookup"><span data-stu-id="bbc66-119">Identity Server with an additional AddApiAuthorization helper method that setups some default ASP.NET Conventions on top of Identity Server.</span></span>
    ```csharp
    services.AddIdentityServer()
      .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```
  * <span data-ttu-id="bbc66-120">Uwierzytelnianie przy użyciu dodatkowej metody pomocnika AddIdentityServerJwt konfiguruje aplikację do weryfikacji tokenów Jwt, generowane przez tożsamość serwera.</span><span class="sxs-lookup"><span data-stu-id="bbc66-120">Authentication with an additional AddIdentityServerJwt helper method that configures the application to validate Jwt tokens produced by Identity Server.</span></span> 
    ```csharp
    services.AddAuthentication()
      .AddIdentityServerJwt();
    ```
* <span data-ttu-id="bbc66-121">Wewnątrz `public void Configure(IApplicationBuilder app)`:</span><span class="sxs-lookup"><span data-stu-id="bbc66-121">Inside `public void Configure(IApplicationBuilder app)`:</span></span>
  * <span data-ttu-id="bbc66-122">Oprogramowanie pośredniczące uwierzytelniania, która jest odpowiedzialna za sprawdzanie poprawności poświadczeń w żądaniu przychodzącym i ustawień użytkownika na kontekst żądania.</span><span class="sxs-lookup"><span data-stu-id="bbc66-122">The authentication middleware that is responsible for validating the credentials in the incoming request and setting the user on the request context.</span></span>
    ```csharp
    app.UseAuthentication();
    ```
  * <span data-ttu-id="bbc66-123">Tożsamość serwera oprogramowania pośredniczącego, które uwidacznia punkty końcowe Open ID Connect.</span><span class="sxs-lookup"><span data-stu-id="bbc66-123">The identity server middleware that exposes the Open ID Connect endpoints.</span></span>
    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="bbc66-124">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="bbc66-124">AddApiAuthorization</span></span> 
<span data-ttu-id="bbc66-125">Ta metoda pomocnika konfiguruje serwer tożsamości do użycia naszych obsługiwanej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-125">This helper method configures Identity Server to use our supported configuration.</span></span> <span data-ttu-id="bbc66-126">Serwer tożsamości jest bardzo wydajna i Rozszerzalna architektura służąca do obsługi aplikacji obawy związane z bezpieczeństwem, ale w tym samym czasie, który udostępnia wiele złożoności, który nie potrzebujemy, aby dowiedzieć się o najbardziej typowych scenariuszy, więc możemy wybrać zestaw konwencje i Opcje konfiguracji, które firma Microsoft uważa, są również dobry punkt wyjścia.</span><span class="sxs-lookup"><span data-stu-id="bbc66-126">Identity Server is a very powerful and extensible framework for handling application security concerns but at the same time that exposes a lot of complexity that we don't need to know about for the most common scenarios, so we choose a set of conventions and configuration options for you that we consider are a good starting point.</span></span> <span data-ttu-id="bbc66-127">Zmian potrzeb w zakresie uwierzytelniania pełnych możliwości serwera tożsamości po nadal dostępne, dzięki czemu możesz dostosować ją do własnych potrzeb.</span><span class="sxs-lookup"><span data-stu-id="bbc66-127">Once your authentication needs change the full power of Identity Server is still available to you so you can customize it to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="bbc66-128">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="bbc66-128">AddIdentityServerJwt</span></span>
<span data-ttu-id="bbc66-129">Ta metoda pomocnika konfiguruje schemat zasad dla aplikacji jako domyślny program obsługi uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="bbc66-129">This helper method configures a policy scheme for the application as the default authentication handler.</span></span> <span data-ttu-id="bbc66-130">Zasada jest skonfigurowana, aby umożliwić tożsamości obsłużenie wszystkich żądań, które przejść do dowolnego podrzędną w obszarze adres url tożsamości "/ tożsamość" i umożliwić JwtBearerHandler wszystkich innych żądaniach obsługi.</span><span class="sxs-lookup"><span data-stu-id="bbc66-130">The policy is configured to let identity handle all the requests that go to any subpath in the Identity url space "/Identity" and to let the JwtBearerHandler handle all other requests.</span></span>
<span data-ttu-id="bbc66-131">Ta metoda rejestruje Addionally `<<ApplicationName>>API` zasobu interfejsu Api za pomocą tożsamości serwera przy użyciu domyślny zakres `<<ApplicationName>>API` i konfiguruje oprogramowanie pośredniczące tokenu JWT Bearer do weryfikacji tokenów wystawionych przez serwer tożsamości dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-131">Addionally this method registers an `<<ApplicationName>>API` Api resource with identity server with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by Identity Server for the application.</span></span>

### <a name="sampledatacontroller"></a><span data-ttu-id="bbc66-132">SampleDataController</span><span class="sxs-lookup"><span data-stu-id="bbc66-132">SampleDataController</span></span>
<span data-ttu-id="bbc66-133">Jeśli spojrzymy na plik Controllers\SampleDataController.cs można zaobserwować `[Authorize]` zastosowany do klasy, która wskazuje, że użytkownik musi być autoryzowane na podstawie domyślnego zasad dostępu do zasobu.</span><span class="sxs-lookup"><span data-stu-id="bbc66-133">If we look at the file Controllers\SampleDataController.cs we can observe the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="bbc66-134">Domyślne zasady autoryzacji, stanie się być skonfigurowana do używania domyślny schemat uwierzytelniania, który jest ustawiony przez `AddIdentityServerJwt` na schemat zasad, które wspomniano powyżej, dzięki czemu program obsługi JwtBearer skonfigurowane przez takie metody pomocnika domyślny program obsługi dla żądania do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-134">The default authorization policy happens to be configured to use the default authentication scheme which is set up by `AddIdentityServerJwt` to the policy scheme that we mentioned above, making the JwtBearer handler configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="bbc66-135">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="bbc66-135">ApplicationDbContext</span></span>
<span data-ttu-id="bbc66-136">Jeśli spojrzymy na plik w Data\ApplicationDbContext.cs widać na tym samym typem DbContext, używamy w tożsamości z wyjątkiem tego, że wykracza poza ApiAuthorizationDbContext (bardziej klasy pochodnej od kontekst IdentityDbContext użytkowników) do uwzględnienia schematu dla tożsamości serwera.</span><span class="sxs-lookup"><span data-stu-id="bbc66-136">If we look at the file in Data\ApplicationDbContext.cs we can see the same DbContext we use in identity with the exception that it extends ApiAuthorizationDbContext (a more derived class from IdentityDbContext) to include the schema for Identity Server.</span></span>
<span data-ttu-id="bbc66-137">Jeśli chcemy pełną kontrolę nad schemat bazy danych możemy po prostu dziedziczą z jednej z dostępnych klas DbContext tożsamości i skonfigurować kontekstu można dołączyć schematu tożsamości przez wywołanie metody `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` na `OnModelCreating` metody.</span><span class="sxs-lookup"><span data-stu-id="bbc66-137">If we want full control of the database schema we can simply inherit from one of the available Identity DbContext classes and configure the context to include the identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="bbc66-138">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="bbc66-138">OidcConfigurationController</span></span>
<span data-ttu-id="bbc66-139">Jeśli przyjrzymy pliku Controllers\OidcConfigurationController.cs widać punkt końcowy tego możemy hali do obsługi parametrów OIDC, które klient musi używać.</span><span class="sxs-lookup"><span data-stu-id="bbc66-139">If we look at the file Controllers\OidcConfigurationController.cs we can see the endpoint that we stand-up to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="bbc66-140">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="bbc66-140">appsettings.json</span></span>
<span data-ttu-id="bbc66-141">Jeśli spojrzymy na plik appsettings.json w folderze głównym projektu, widać nową `IdentityServer` sekcja, która opisuje listę skonfigurowane klientów i widać, że istnieje jednego klienta.</span><span class="sxs-lookup"><span data-stu-id="bbc66-141">If we look at the appsettings.json file on the root of the project, we can see a new `IdentityServer` section that describes the list of configured clients and we can see that there is a single client.</span></span> <span data-ttu-id="bbc66-142">Nazwa klienta odpowiada nazwę aplikacji i jest mapowany zgodnie z Konwencją do parametru identyfikatora klienta oAuth.</span><span class="sxs-lookup"><span data-stu-id="bbc66-142">The name of the client corresponds to the name of the application and is mapped by convention to the oAuth ClientId parameter.</span></span> <span data-ttu-id="bbc66-143">Profil, który wskazuje, jakiego rodzaju aplikacji, które będziemy konfigurować, a firma Microsoft wewnętrznie Użyj dysku konwencjami, które upraszczają proces konfiguracji dla serwera.</span><span class="sxs-lookup"><span data-stu-id="bbc66-143">The profile indicates what type of application we are configuring, and we use it internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="bbc66-144">Istnieje kilka profilów dostępnych wyjaśnione w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="bbc66-144">There are several profiles available explained in the section below.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="bbc66-145">appsettings.Development.json</span><span class="sxs-lookup"><span data-stu-id="bbc66-145">appsettings.Development.json</span></span>
<span data-ttu-id="bbc66-146">Jeśli przyjrzymy się appsettings. Development.JSON pliku w katalogu głównym projektu widać nową `IdentityServer` sekcja, która opisuje klucz, firma Microsoft jest używany do podpisywania tokenów.</span><span class="sxs-lookup"><span data-stu-id="bbc66-146">If we look at the appsettings.Development.json file on the root of the project, we can see a new `IdentityServer` section that describes the key we are using to sign tokens.</span></span> <span data-ttu-id="bbc66-147">Podczas wdrażania do produkcji klucz musi zostać aprowizowane i wdrażana wraz z aplikacji, co zostało opisane poniżej.</span><span class="sxs-lookup"><span data-stu-id="bbc66-147">When deploying to production a key needs to be provisioned and deployed alongside the application as explained below.</span></span>

```json
  "IdentityServer": {
    "Key": {
      "Type": "Development"
    }
  }
}
```

## <a name="general-description-of-the-angular-application"></a><span data-ttu-id="bbc66-148">Ogólny opis aplikację platformy Angular</span><span class="sxs-lookup"><span data-stu-id="bbc66-148">General description of the Angular application</span></span>
<span data-ttu-id="bbc66-149">Obsługa uwierzytelniania i autoryzacji interfejsu API w szablonie usługi Angular znajduje się w jego własnej moduł Angular.</span><span class="sxs-lookup"><span data-stu-id="bbc66-149">The support for authentication and API authorization in the Angular template lives in its own Angular module.</span></span> <span data-ttu-id="bbc66-150">W obszarze ClientApp\src\api autoryzacji i składa się z następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="bbc66-150">Under ClientApp\src\api-authorization and it is composed of the following elements:</span></span>
* <span data-ttu-id="bbc66-151">Składniki 3:</span><span class="sxs-lookup"><span data-stu-id="bbc66-151">3 Components:</span></span>
  * <span data-ttu-id="bbc66-152">Składnik nazwy logowania: Obsługuje przepływu logowania dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-152">Login component: Handles the login flow for the app.</span></span>
  * <span data-ttu-id="bbc66-153">Składnik wylogowania: Obsługuje przepływu wylogowania w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-153">Logout component: Handles the logout flow for the app.</span></span>
  * <span data-ttu-id="bbc66-154">Element menu logowania: Element widget, który wyświetla nazwy aktualnie uwierzytelnionego użytkownika wraz z łączami do zarządzania profilu użytkownika i wylogować się lub łączy się zalogować się lub Zarejestruj się, gdy użytkownik nie jest uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="bbc66-154">Login menu component: A widget that displays the current authenticated user with links to manage the user profile and log out or links to log in or register when the user is not authenticated.</span></span>
* <span data-ttu-id="bbc66-155">Strażnik trasy `AuthorizeGuard` , mogą być dodawane do trasy i wymaga od użytkownika uwierzytelniali się przed odwiedzający trasy.</span><span class="sxs-lookup"><span data-stu-id="bbc66-155">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="bbc66-156">Interceptor http `AuthorizeInterceptor` , dołącza token dostępu na wychodzące żądania HTTP, przeznaczone dla interfejsu API, gdy użytkownik jest uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="bbc66-156">An http interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="bbc66-157">Usługa `AuthorizeService` który obsłuży niższym poziomie procesu uwierzytelniania i udostępnia informacje o uwierzytelnionym użytkowniku w pozostałej części aplikacji do użycia.</span><span class="sxs-lookup"><span data-stu-id="bbc66-157">A service `AuthorizeService` that handles the lower level details of the authentication process and exposes information about the authenticated user to the rest of the application for consumption.</span></span>
* <span data-ttu-id="bbc66-158">Moduł angular, który definiuje tras skojarzone z części uwierzytelniania aplikacji i udostępnia składnika menu logowania, interceptor, osłony i usługi do użycia z pozostałymi aplikacjami.</span><span class="sxs-lookup"><span data-stu-id="bbc66-158">An angular module that defines routes associated with the authentication parts of the application and exposes the login menu component, the interceptor, the guard and the service for consumption from the rest of the application.</span></span>

## <a name="general-description-of-the-react-application"></a><span data-ttu-id="bbc66-159">Ogólny opis aplikacji platformy React</span><span class="sxs-lookup"><span data-stu-id="bbc66-159">General description of the React application</span></span>
<span data-ttu-id="bbc66-160">Obsługa uwierzytelniania i autoryzacji interfejsu API w życie szablonu platformy React w obszarze ClientApp\src\components\api authorization\ i składa się z następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="bbc66-160">The support for authentication and API authorization in the React template lives under ClientApp\src\components\api-authorization\ and it is composed of the following elements:</span></span>
* <span data-ttu-id="bbc66-161">4 składniki:</span><span class="sxs-lookup"><span data-stu-id="bbc66-161">4 Components:</span></span>
  * <span data-ttu-id="bbc66-162">Składnik nazwy logowania: Obsługuje przepływu logowania dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-162">Login component: Handles the login flow for the app.</span></span>
  * <span data-ttu-id="bbc66-163">Składnik wylogowania: Obsługuje przepływu wylogowania w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-163">Logout component: Handles the logout flow for the app.</span></span>
  * <span data-ttu-id="bbc66-164">Element menu logowania: Element widget, który wyświetla nazwy aktualnie uwierzytelnionego użytkownika wraz z łączami do zarządzania profilu użytkownika i wylogować się lub łączy się zalogować się lub Zarejestruj się, gdy użytkownik nie jest uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="bbc66-164">Login menu component: A widget that displays the current authenticated user with links to manage the user profile and log out or links to log in or register when the user is not authenticated.</span></span>
  * <span data-ttu-id="bbc66-165">AuthorizeRoute: Składnik trasy, który wymaga od użytkownika uwierzytelniali się przed renderowaniem składnika wskazanego w parametrze składnika.</span><span class="sxs-lookup"><span data-stu-id="bbc66-165">AuthorizeRoute: A route component that requires a user to be authenticated before rendering the component indicated in the Component parameter.</span></span>
  * <span data-ttu-id="bbc66-166">Wyeksportowane `authService` wystąpienia klasy `AuthorizeService` który obsłuży niższym poziomie procesu uwierzytelniania i udostępnia informacje o uwierzytelnionym użytkowniku w pozostałej części aplikacji do użycia.</span><span class="sxs-lookup"><span data-stu-id="bbc66-166">An exported `authService` instance of class `AuthorizeService` that handles the lower level details of the authentication process and exposes information about the authenticated user to the rest of the application for consumption.</span></span>

<span data-ttu-id="bbc66-167">Teraz, w związku z czym dostrzegliśmy głównymi składnikami rozwiązania, firma Microsoft może Przyjrzyj się określonych poszczególne scenariusze dla aplikacji:</span><span class="sxs-lookup"><span data-stu-id="bbc66-167">Now that we've seen the main components of the solution, we can take a specific look at individual scenarios for the application:</span></span>

## <a name="requiring-authorization-on-a-new-api"></a><span data-ttu-id="bbc66-168">Wymaganie autoryzacji na nowy interfejs API</span><span class="sxs-lookup"><span data-stu-id="bbc66-168">Requiring authorization on a new API</span></span>
<span data-ttu-id="bbc66-169">System jest skonfigurowany gotowych umożliwiają proste do wymagają autoryzacji dla nowych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="bbc66-169">The system is configured out of the box to make it trivial to require authorization for new APIs.</span></span> <span data-ttu-id="bbc66-170">Aby to zrobić, po prostu Utwórz nowy kontroler i Dodaj `[Authorize]` atrybutów do klasy kontrolera lub dowolnych akcji w obrębie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="bbc66-170">In order to do so, simply create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protecting-a-client-side-route-angular"></a><span data-ttu-id="bbc66-171">Ochrona trasę po stronie klienta (Angular)</span><span class="sxs-lookup"><span data-stu-id="bbc66-171">Protecting a client-side route (Angular)</span></span>
<span data-ttu-id="bbc66-172">Ochrona trasy po stronie klienta odbywa się przez dodanie guard autoryzacji do listy osłony do uruchomienia podczas konfigurowania trasy.</span><span class="sxs-lookup"><span data-stu-id="bbc66-172">Protecting a client side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="bbc66-173">Na przykład możesz zobaczyć, jak trasa pobierania danych jest skonfigurowana w module usługi angular głównej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="bbc66-173">As an example you can see how the fetch-data route is configured within the main app angular module:</span></span>

```ts
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="bbc66-174">Ważne jest, aby wspomnieć, że ochrona trasy nie chroni istniejący punkt końcowy (który nadal wymaga `[Authorize]` zastosować atrybut), ale że go tylko uniemożliwia użytkownikowi przejście do trasy po stronie danego klienta, gdy nie jest uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="bbc66-174">It is important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client side route when it is not authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="bbc66-175">Uwierzytelnianie żądań interfejsu API (Angular)</span><span class="sxs-lookup"><span data-stu-id="bbc66-175">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="bbc66-176">Uwierzytelniania żądań do interfejsów API hostowanych wzdłuż krawędzi, że aplikacja odbywa się automatycznie przy użyciu interceptor klienta HTTP, które są definiowane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="bbc66-176">Authenticating requests to APIs hosted along side the application is done automatically through the use of the HTTP client interceptor defined by the application.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="bbc66-177">Ochrona trasę po stronie klienta (React)</span><span class="sxs-lookup"><span data-stu-id="bbc66-177">Protect a client-side route (React)</span></span>

<span data-ttu-id="bbc66-178">Ochrona trasy po stronie klienta odbywa się za pomocą składnika AuthorizeRoute zamiast zwykłego składnika trasy.</span><span class="sxs-lookup"><span data-stu-id="bbc66-178">Protecting a client side route is done by using the AuthorizeRoute component instead of the plain Route component.</span></span> <span data-ttu-id="bbc66-179">Na przykład możesz zobaczyć, jak trasa pobierania danych jest skonfigurowana w składniku aplikacji:</span><span class="sxs-lookup"><span data-stu-id="bbc66-179">As an example you can see how the fetch-data route is configured within the App component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="bbc66-180">Ważne jest, aby wspomnieć, że ochrona trasy nie chroni istniejący punkt końcowy (który nadal wymaga `[Authorize]` zastosować atrybut), ale że go tylko uniemożliwia użytkownikowi przejście do trasy po stronie danego klienta, gdy nie jest uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="bbc66-180">It is important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client side route when it is not authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="bbc66-181">Uwierzytelnianie żądań interfejsu API (React)</span><span class="sxs-lookup"><span data-stu-id="bbc66-181">Authenticate API requests (React)</span></span>

<span data-ttu-id="bbc66-182">Uwierzytelnianie żądań przy użyciu platformy react odbywa się przez importowanie pierwszego `authService` wystąpienia z `AuthorizeService` pobierania tokenu dostępu z authService i dołączania ich do żądania, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="bbc66-182">Authenticating requests with react is done by first importing the `authService` instance from the `AuthorizeService` and then retrieving the access token from the authService and attaching it to the request as shown below.</span></span> <span data-ttu-id="bbc66-183">Składniki platformy react zazwyczaj jest to wykonywane w metodzie cyklu życia componentDidMount lub w wyniku z interakcji z użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="bbc66-183">In react components this is typically done in the componentDidMount lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="bbc66-184">Importowanie authService do składnika</span><span class="sxs-lookup"><span data-stu-id="bbc66-184">Import the authService into your component</span></span>

```js
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="bbc66-185">Pobieranie i dołączanie token dostępu do odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="bbc66-185">Retrieve and attach the access token to the response</span></span>

```js
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-into-production"></a><span data-ttu-id="bbc66-186">Wdrażanie w środowisku produkcyjnym</span><span class="sxs-lookup"><span data-stu-id="bbc66-186">Deploy into production</span></span>

<span data-ttu-id="bbc66-187">Aby wdrożyć aplikację w środowisku produkcyjnym należy aprowizować kilka zasobów:</span><span class="sxs-lookup"><span data-stu-id="bbc66-187">In order to deploy the application into production we need to provision several resources:</span></span>
* <span data-ttu-id="bbc66-188">Przyznaje bazę danych do przechowywania tożsamości konta użytkowników i serwera tożsamości.</span><span class="sxs-lookup"><span data-stu-id="bbc66-188">A database to store the Identity user accounts and the identity server grants.</span></span>
* <span data-ttu-id="bbc66-189">Certyfikatu produkcyjnego do użycia podczas podpisywania tokenów.</span><span class="sxs-lookup"><span data-stu-id="bbc66-189">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="bbc66-190">Nie istnieją określone wymagania dotyczące tego certyfikatu; może być certyfikat z podpisem własnym lub certyfikatu dostarczanymi za pośrednictwem urzędu certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-190">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="bbc66-191">Mogą być generowane przy użyciu standardowych narzędzi, takich jak program powershell lub protokołu openssl.</span><span class="sxs-lookup"><span data-stu-id="bbc66-191">It can be generated through standard tools like powershell or openssl.</span></span>
  * <span data-ttu-id="bbc66-192">Może być zainstalowany w magazynie certyfikatów na komputerach docelowych lub wdrożony jako plik pfx z silnym hasłem.</span><span class="sxs-lookup"><span data-stu-id="bbc66-192">It can be installed into the certificate store on the target machines or deployed as a pfx file with a strong password.</span></span>

### <a name="example-deploy-into-azure-websites"></a><span data-ttu-id="bbc66-193">Przykład: Wdrażanie w usłudze Azure Websites</span><span class="sxs-lookup"><span data-stu-id="bbc66-193">Example: Deploy into Azure Websites</span></span>

<span data-ttu-id="bbc66-194">W tej sekcji użyjemy do wdrożenia aplikacji w usłudze Azure websites przy użyciu certyfikatu przechowywanego w magazynie certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="bbc66-194">In this section we are going to deploy the application to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="bbc66-195">Należy zmodyfikować aplikację, aby załadować talonu z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="bbc66-195">We need to modify the application to load a certicate from the certificate store.</span></span> <span data-ttu-id="bbc66-196">Aby to zrobić, nasz plan usługi app service musi mieć co najmniej w warstwie standardowa, jeśli skonfigurowaną w późniejszym kroku.</span><span class="sxs-lookup"><span data-stu-id="bbc66-196">To do so, our app service plan needs to be at least on the standard tier when we configure in a later step.</span></span> <span data-ttu-id="bbc66-197">W naszej aplikacji po prostu należy zmodyfikować sekcję IdentityServer appsettings.json obejmujący istotne szczegóły:</span><span class="sxs-lookup"><span data-stu-id="bbc66-197">In our application we simply need to modify the IdentityServer section on appsettings.json to include the key details:</span></span>
```json
  "IdentityServer": {
    "Key": {
      "Type": "Store",
      "StoreName": "My",
      "StoreLocation": "CurrentUser",
      "Name": "CN=MyApplication"
    }
  }
}
```
* <span data-ttu-id="bbc66-198">Właściwość Nazwa certyfikatu odpowiada wyróżniająca podmiotu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="bbc66-198">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="bbc66-199">Lokalizacja magazynu reprezentuje, gdzie można załadować certyfikatu (CurrentUrser lub LocalMachine).</span><span class="sxs-lookup"><span data-stu-id="bbc66-199">The store location represents where to load the certificate from (CurrentUrser or LocalMachine).</span></span>
* <span data-ttu-id="bbc66-200">Nazwa magazynu reprezentuje nazwę magazynu certyfikatów, gdy certyfikat jest przechowywany, w tym przypadku, który wskazuje w magazynie osobistym użytkownika.</span><span class="sxs-lookup"><span data-stu-id="bbc66-200">The store name represents the name of the certificate store where the certificate is stored, in this case it points to the personal user store.</span></span>

<span data-ttu-id="bbc66-201">Aby wdrożyć w usłudze Azure Websites, wdrażanie aplikacji, wykonaj czynności w [wdrażanie aplikacji na platformie Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) zasobów platformy Azure niezbędnych do tworzenia i wdrażania aplikacji w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="bbc66-201">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="bbc66-202">Po wykonaniu tego, aplikacja jest wdrażana na platformie Azure, ale nie jest jeszcze całkowicie funkcjonalności nadal trzeba skonfigurować certyfikat używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="bbc66-202">After doing this, the application is deployed into Azure but is not yet completely functional as we still need to setup the certificate to be used by the application.</span></span> <span data-ttu-id="bbc66-203">Aby to zrobić, należy mieć odcisk palca certyfikatu, będziemy używać, i wykonaj kroki opisane w [ładowanie certyfikatów](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span><span class="sxs-lookup"><span data-stu-id="bbc66-203">To do so, we need to have the thumbprint for the certificate we are going to use and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span></span>

<span data-ttu-id="bbc66-204">Chociaż te kroki wspomina o identyfikatorach protokołu SSL, jest sekcja "Certyfikaty prywatne" w portalu, gdzie można przekazać nasze elastycznie certyfikatu za pomocą naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-204">While these steps mention SSL, there is a "Private certificates" section on the portal where we can upload our provisioned certificate to use with our app.</span></span>

<span data-ttu-id="bbc66-205">Po wykonaniu tego kroku będziemy mogli ponownie uruchomić naszą aplikację i powinna być w pełni funkcjonalna.</span><span class="sxs-lookup"><span data-stu-id="bbc66-205">After this step, we should be able to restart our application and it should be completely functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="bbc66-206">Inne opcje konfiguracji</span><span class="sxs-lookup"><span data-stu-id="bbc66-206">Other configuration options</span></span>
<span data-ttu-id="bbc66-207">Nasze wsparcie dla interfejsu API autoryzacji opartą na tożsamość serwera z zestawem Konwencji, wartości domyślne i ulepszenia, aby uprościć środowisko dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-207">Our support for API authorization builds on top of Identity Server with a set of conventions, default values and enhancements to simplify the experience for Single Page Applications.</span></span> <span data-ttu-id="bbc66-208">Needless, że pełnych możliwości serwera tożsamości jest dostępna w tle integracji, które firma Microsoft oferuje nie obejmuje Twojego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="bbc66-208">Needless to say, the full power of Identity Server is available behind the scenes if the integrations that we offer don't cover your scenario.</span></span> <span data-ttu-id="bbc66-209">Nasze wsparcie koncentruje się na tak zwany "firmy Microsoft" aplikacji, gdzie wszystkie aplikacje są tworzone i wdrażane przez naszej organizacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-209">Our support is focused on what we call "first-party" applications, where all the applications are created and deployed by our organization.</span></span> <span data-ttu-id="bbc66-210">Jako takie firma Microsoft nie oferuje wsparcie dla elementów, takich jak zgody lub federacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-210">As such we don't offer support for things like consent or federation.</span></span> <span data-ttu-id="bbc66-211">W tych scenariuszach naszych zaleca się używania tożsamości serwera i postępuj zgodnie z ich dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-211">For those scenarios our recommendation is to use Identity Server and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="bbc66-212">Profile aplikacji</span><span class="sxs-lookup"><span data-stu-id="bbc66-212">Application profiles</span></span>
<span data-ttu-id="bbc66-213">Profile aplikacji są wstępnie zdefiniowane konfiguracje dla aplikacji, które uściślić swoich parametrów.</span><span class="sxs-lookup"><span data-stu-id="bbc66-213">Application profiles are predefined configurations for applications that further define their parameters.</span></span> <span data-ttu-id="bbc66-214">Teraz obsługujemy dwa profile:</span><span class="sxs-lookup"><span data-stu-id="bbc66-214">At this time we support two profiles:</span></span>
* <span data-ttu-id="bbc66-215">IdentityServerSPA: Reprezentuje aplikacji jednostronicowej hostowana razem tożsamości serwera jako pojedynczą jednostką.</span><span class="sxs-lookup"><span data-stu-id="bbc66-215">IdentityServerSPA: Represents a single page application hosted alongside Identity Server as a single unit.</span></span>
  * <span data-ttu-id="bbc66-216">Wartością domyślną jest redirect_uri `/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="bbc66-216">The redirect_uri defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="bbc66-217">Wartością domyślną jest post_logout_redirect_uri `/authentication/logout-callback`.</span><span class="sxs-lookup"><span data-stu-id="bbc66-217">The post_logout_redirect_uri defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="bbc66-218">Obejmuje zestaw zakresów `openid`, `profile`i każdy zakres zdefiniowany dla interfejsów API w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-218">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the application.</span></span>
  * <span data-ttu-id="bbc66-219">Zestaw dozwolonych typów odpowiedzi OIDC jest `id_token token` lub każdego z nich osobno (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="bbc66-219">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="bbc66-220">Tryb odpowiedzi dozwolone jest `fragment`.</span><span class="sxs-lookup"><span data-stu-id="bbc66-220">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="bbc66-221">SPA: Reprezentuje aplikacja jednostronicowa, który nie jest obsługiwany za pomocą tożsamości serwera.</span><span class="sxs-lookup"><span data-stu-id="bbc66-221">SPA: Represents a single page application that is not hosted with Identity Server.</span></span>
  * <span data-ttu-id="bbc66-222">Obejmuje zestaw zakresów `openid`, `profile`i każdy zakres zdefiniowany dla interfejsów API w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-222">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the application.</span></span>
  * <span data-ttu-id="bbc66-223">Zestaw dozwolonych typów odpowiedzi OIDC jest `id_token token` lub każdego z nich osobno (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="bbc66-223">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="bbc66-224">Tryb odpowiedzi dozwolone jest `fragment`.</span><span class="sxs-lookup"><span data-stu-id="bbc66-224">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="bbc66-225">IdentityServerJwt: Reprezentuje interfejs API, który znajduje się wraz z serwerem tożsamości.</span><span class="sxs-lookup"><span data-stu-id="bbc66-225">IdentityServerJwt: Represents an API that is hosted alongside with Identity Server.</span></span>
  * <span data-ttu-id="bbc66-226">Aplikacja jest skonfigurowana do mieć pojedynczy zakres, która domyślnie używa nazwy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-226">The application is configured to have a single scope that defaults to the application name.</span></span>
* <span data-ttu-id="bbc66-227">API: Reprezentuje interfejs API, który nie jest obsługiwana za pomocą tożsamości serwera.</span><span class="sxs-lookup"><span data-stu-id="bbc66-227">API: Represents an API that is not hosted with Identity Server.</span></span>
  * <span data-ttu-id="bbc66-228">Aplikacja jest skonfigurowana do mieć pojedynczy zakres, która domyślnie używa nazwy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbc66-228">The application is configured to have a single scope that defaults to the application name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="bbc66-229">Konfigurowanie za pomocą AppSettings</span><span class="sxs-lookup"><span data-stu-id="bbc66-229">Configuration through AppSettings</span></span>
<span data-ttu-id="bbc66-230">Firma Microsoft można skonfigurować za pomocą nasz system konfiguracji aplikacji, dodając je do listy klientów lub zasoby odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="bbc66-230">We can configure the applications through our configuration system by adding them to the list of Clients or Resources respectively.</span></span> 

<span data-ttu-id="bbc66-231">Podczas konfigurowania klientów można skonfigurować `redirect_uri` i `post_logout_redirect_uri` jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="bbc66-231">When configuring clients we can configure the `redirect_uri` and the `post_logout_redirect_uri` as shown below:</span></span>
```json
  "IdentityServer": {
    "Clients": {
      "MySPA": {
        "Profile": "SPA",
        "RedirectUri": "https://www.example.com/authentication/login-callback",
        "LogoutUri": "https://www.example.com/authentication/logout-callback"
      }
    }
  }
```

<span data-ttu-id="bbc66-232">Podczas konfigurowania zasobów możemy skonfigurować zakresy dla zasobu, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="bbc66-232">When configuring resources we can configure the scopes for the resource as shown below:</span></span>
```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c",
    }
  }
}
```

### <a name="configuration-through-code"></a><span data-ttu-id="bbc66-233">Konfigurowanie za pomocą kodu</span><span class="sxs-lookup"><span data-stu-id="bbc66-233">Configuration through code</span></span>
<span data-ttu-id="bbc66-234">Firma Microsoft można również skonfigurować klientów i zasobów za pomocą kodu za pomocą przeciążenia AddApiAuthorization, które wykonują akcję, aby skonfigurować opcje.</span><span class="sxs-lookup"><span data-stu-id="bbc66-234">We can also configure the clients and resources through code using an overload of AddApiAuthorization that takes an action to configure options.</span></span>
```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA",
        spa => spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
            .WithLogoutRedirectUri("http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource => resource.WithScopes("a", "b", "c"));
});
```
