---
title: Wprowadzenie do uwierzytelniania dla pojedynczej aplikacji strony programu ASP.NET Core
author: javiercn
description: Użyj tożsamości z jednej strony aplikacji hostowaną wewnątrz aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/07/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 4afc9ac0a3c54b452c6a1b23e4de31d7e2fc5284
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665340"
---
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="c859d-103">Uwierzytelnianie i autoryzację dla aplikacji jednostronicowych</span><span class="sxs-lookup"><span data-stu-id="c859d-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="c859d-104">Platforma ASP.NET Core w wersji 3.0 lub nowszej oferuje uwierzytelnianie w jednej strony aplikacji (aplikacji jednostronicowych) przy użyciu obsługi interfejsu API autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-104">ASP.NET Core 3.0 or later offers authentication in Single Page Apps (SPAs) using the support for API authorization.</span></span> <span data-ttu-id="c859d-105">Tożsamości platformy ASP.NET Core, uwierzytelniania i przechowywanie użytkowników jest połączony z [IdentityServer](https://identityserver.io/) implementowania Open ID Connect.</span><span class="sxs-lookup"><span data-stu-id="c859d-105">ASP.NET Core Identity for authenticating and storing users is combined with [IdentityServer](https://identityserver.io/) for implementing Open ID Connect.</span></span>

<span data-ttu-id="c859d-106">Parametr uwierzytelniania został dodany do **Angular** i **React** projektu szablonów, które są podobne do parametru uwierzytelniania w **aplikacji sieci Web (Model-View-Controller)**  (MVC) i **aplikacji sieci Web** szablony projektów (Razor strony).</span><span class="sxs-lookup"><span data-stu-id="c859d-106">An authentication parameter was added to the **Angular** and **React** project templates that is similar to the authentication parameter in the **Web Application (Model-View-Controller)** (MVC) and **Web Application** (Razor Pages) project templates.</span></span> <span data-ttu-id="c859d-107">Wartości parametrów dozwolone są **Brak** i **poszczególnych**.</span><span class="sxs-lookup"><span data-stu-id="c859d-107">The allowed parameter values are **None** and **Individual**.</span></span> <span data-ttu-id="c859d-108">**React.js i kontenera Redux** szablonu projektu nie obsługuje parametru uwierzytelniania w tej chwili.</span><span class="sxs-lookup"><span data-stu-id="c859d-108">The **React.js and Redux** project template doesn't support the authentication parameter at this time.</span></span>

## <a name="create-an-app-with-api-authorization-support"></a><span data-ttu-id="c859d-109">Tworzenie aplikacji dzięki obsłudze autoryzacji interfejsu API</span><span class="sxs-lookup"><span data-stu-id="c859d-109">Create an app with API authorization support</span></span>

<span data-ttu-id="c859d-110">Przy użyciu usług Angular i reagowania aplikacji jednostronicowych można uwierzytelniania i autoryzacji użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c859d-110">User authentication and authorization can be used with both Angular and React SPAs.</span></span> <span data-ttu-id="c859d-111">Otwórz powłokę wiersza polecenia, a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="c859d-111">Open a command shell, and run the following command:</span></span>

<span data-ttu-id="c859d-112">**Angular**:</span><span class="sxs-lookup"><span data-stu-id="c859d-112">**Angular**:</span></span>

```console
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="c859d-113">**React**:</span><span class="sxs-lookup"><span data-stu-id="c859d-113">**React**:</span></span>

```console
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="c859d-114">Poprzednie polecenie umożliwia utworzenie aplikacji platformy ASP.NET Core za pomocą *ClientApp* katalog zawierający SPA.</span><span class="sxs-lookup"><span data-stu-id="c859d-114">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the SPA.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="c859d-115">Ogólny opis składników platformy ASP.NET Core w aplikacji</span><span class="sxs-lookup"><span data-stu-id="c859d-115">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="c859d-116">W poniższych sekcjach opisano ma zostać dodany do projektu, gdy uwierzytelnianie pomoc techniczna jest dostępna:</span><span class="sxs-lookup"><span data-stu-id="c859d-116">The following sections describe additions to the project when authentication support is included:</span></span>

### <a name="startup-class"></a><span data-ttu-id="c859d-117">Klasa początkowa</span><span class="sxs-lookup"><span data-stu-id="c859d-117">Startup class</span></span>

<span data-ttu-id="c859d-118">`Startup` Klasa ma następującymi dodatkami:</span><span class="sxs-lookup"><span data-stu-id="c859d-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="c859d-119">Wewnątrz `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="c859d-119">Inside the `Startup.ConfigureServices` method:</span></span>
  * <span data-ttu-id="c859d-120">Tożsamość przy użyciu domyślnego interfejsu użytkownika:</span><span class="sxs-lookup"><span data-stu-id="c859d-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="c859d-121">IdentityServer przy użyciu dodatkowego `AddApiAuthorization` metody pomocnika tej konfiguracji niektóre domyślne konwencje platformy ASP.NET Core na podstawie IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="c859d-121">IdentityServer with an additional `AddApiAuthorization` helper method that setups some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="c859d-122">Uwierzytelnianie przy użyciu dodatkowego `AddIdentityServerJwt` metody pomocnika, która umożliwia skonfigurowanie aplikacji, aby sprawdzał poprawność tokenów JWT produkowane przez IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="c859d-122">Authentication with an additional `AddIdentityServerJwt` helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="c859d-123">Wewnątrz `Startup.Configure` metody:</span><span class="sxs-lookup"><span data-stu-id="c859d-123">Inside the `Startup.Configure` method:</span></span>
  * <span data-ttu-id="c859d-124">Oprogramowanie pośredniczące uwierzytelniania, która jest odpowiedzialna za sprawdzanie poprawności poświadczeń żądania i ustawień użytkownika na kontekst żądania:</span><span class="sxs-lookup"><span data-stu-id="c859d-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="c859d-125">Oprogramowanie pośredniczące IdentityServer, który uwidacznia punkty końcowe Open ID Connect:</span><span class="sxs-lookup"><span data-stu-id="c859d-125">The IdentityServer middleware that exposes the Open ID Connect endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="c859d-126">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="c859d-126">AddApiAuthorization</span></span>

<span data-ttu-id="c859d-127">Ta metoda pomocnika konfiguruje IdentityServer używania naszych obsługiwanej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c859d-127">This helper method configures IdentityServer to use our supported configuration.</span></span> <span data-ttu-id="c859d-128">IdentityServer to wydajna i rozszerzalna struktura do obsługi aplikacji obawy związane z bezpieczeństwem.</span><span class="sxs-lookup"><span data-stu-id="c859d-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="c859d-129">W tym samym czasie, który udostępnia niepotrzebne złożoność najbardziej typowych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="c859d-129">At the same time, that exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="c859d-130">W związku z tym zestaw opcji konfiguracji i konwencje są dostępne, jeśli są one dobry punkt wyjścia.</span><span class="sxs-lookup"><span data-stu-id="c859d-130">Consequently, a set of conventions and configuration options is provided to you that are considered a good starting point.</span></span> <span data-ttu-id="c859d-131">Po zmian potrzeb w zakresie uwierzytelniania, pełnych możliwości IdentityServer jest nadal dostępne dostosować uwierzytelniania do własnych potrzeb.</span><span class="sxs-lookup"><span data-stu-id="c859d-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="c859d-132">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="c859d-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="c859d-133">Ta metoda pomocnika konfiguruje schemat zasad dla aplikacji jako domyślny program obsługi uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="c859d-133">This helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="c859d-134">Zasada jest skonfigurowana, aby umożliwić tożsamości obsługuje wszystkie żądania kierowane do dowolnego podrzędną w obszarze adres URL tożsamości "/ tożsamość".</span><span class="sxs-lookup"><span data-stu-id="c859d-134">The policy is configured to let Identity handle all requests routed to any subpath in the Identity URL space "/Identity".</span></span> <span data-ttu-id="c859d-135">`JwtBearerHandler` Obsługuje wszystkie inne żądania.</span><span class="sxs-lookup"><span data-stu-id="c859d-135">The `JwtBearerHandler` handles all other requests.</span></span> <span data-ttu-id="c859d-136">Ponadto ta metoda rejestruje `<<ApplicationName>>API` zasobach interfejsu API za pomocą IdentityServer z domyślny zakres `<<ApplicationName>>API` i konfiguruje oprogramowanie pośredniczące tokenu JWT Bearer do zweryfikowania tokeny wystawione przez IdentityServer dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-136">Additionally, this method registers an `<<ApplicationName>>API` API resource with IdentityServer with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="sampledatacontroller"></a><span data-ttu-id="c859d-137">SampleDataController</span><span class="sxs-lookup"><span data-stu-id="c859d-137">SampleDataController</span></span>

<span data-ttu-id="c859d-138">W *Controllers\SampleDataController.cs* plików, zwróć uwagę, `[Authorize]` zastosowany do klasy, która wskazuje, że użytkownik musi być autoryzowane na podstawie domyślnego zasad dostępu do zasobu.</span><span class="sxs-lookup"><span data-stu-id="c859d-138">In the *Controllers\SampleDataController.cs* file, notice the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="c859d-139">Domyślne zasady autoryzacji, stanie się być skonfigurowana do używania domyślnego schematu uwierzytelniania, który jest ustawiony przez `AddIdentityServerJwt` na schemat zasad, które wspomniano powyżej, dzięki czemu `JwtBearerHandler` skonfigurowane przez takie metody pomocnika domyślny program obsługi dla żądania do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-139">The default authorization policy happens to be configured to use the default authentication scheme, which is set up by `AddIdentityServerJwt` to the policy scheme that was mentioned above, making the `JwtBearerHandler` configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="c859d-140">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="c859d-140">ApplicationDbContext</span></span>

<span data-ttu-id="c859d-141">W *Data\ApplicationDbContext.cs* plików, zwróć uwagę, taka sama `DbContext` jest używany w tożsamości z wyjątkiem, że rozciąga `ApiAuthorizationDbContext` (bardziej pochodne klasy z `IdentityDbContext`) aby objąć IdentityServer schematu.</span><span class="sxs-lookup"><span data-stu-id="c859d-141">In the *Data\ApplicationDbContext.cs* file, notice the same `DbContext` is used in Identity with the exception that it extends `ApiAuthorizationDbContext` (a more derived class from `IdentityDbContext`) to include the schema for IdentityServer.</span></span>

<span data-ttu-id="c859d-142">Aby uzyskać pełną kontrolę nad schemat bazy danych, dziedziczą z jednej z dostępnych tożsamości `DbContext` klasy i skonfigurować kontekstu można dołączyć schematu tożsamości przez wywołanie metody `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` na `OnModelCreating` metody.</span><span class="sxs-lookup"><span data-stu-id="c859d-142">To gain full control of the database schema, inherit from one of the available Identity `DbContext` classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="c859d-143">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="c859d-143">OidcConfigurationController</span></span>

<span data-ttu-id="c859d-144">W *Controllers\OidcConfigurationController.cs* plików, zwróć uwagę, punkt końcowy, który jest przygotowany do obsługi parametrów OIDC, które klient musi używać.</span><span class="sxs-lookup"><span data-stu-id="c859d-144">In the *Controllers\OidcConfigurationController.cs* file, notice the endpoint that's provisioned to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="c859d-145">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="c859d-145">appsettings.json</span></span>

<span data-ttu-id="c859d-146">W *appsettings.json* pliku głównego projektu jest nowym `IdentityServer` sekcja, która opisuje listę skonfigurować klientów.</span><span class="sxs-lookup"><span data-stu-id="c859d-146">In the *appsettings.json* file of the project root, there's a new `IdentityServer` section that describes the list of configured clients.</span></span> <span data-ttu-id="c859d-147">W poniższym przykładzie są jednego klienta.</span><span class="sxs-lookup"><span data-stu-id="c859d-147">In the following example, there's a single client.</span></span> <span data-ttu-id="c859d-148">Nazwa klienta odpowiada nazwie aplikacji i jest mapowany przez Konwencję w celu uwierzytelniania OAuth `ClientId` parametru.</span><span class="sxs-lookup"><span data-stu-id="c859d-148">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="c859d-149">Profil, który wskazuje typ aplikacji jest skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="c859d-149">The profile indicates the app type being configured.</span></span> <span data-ttu-id="c859d-150">Jest ona używana wewnętrznie w celu konwencje dysku, które upraszczają proces konfiguracji dla serwera.</span><span class="sxs-lookup"><span data-stu-id="c859d-150">It's used internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="c859d-151">Brak dostępnych kilka profilów, jak wyjaśniono w [profile aplikacji](#application-profiles) sekcji.</span><span class="sxs-lookup"><span data-stu-id="c859d-151">There are several profiles available, as explained in the [Application profiles](#application-profiles) section.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="c859d-152">appsettings.Development.json</span><span class="sxs-lookup"><span data-stu-id="c859d-152">appsettings.Development.json</span></span>

<span data-ttu-id="c859d-153">W *appsettings. Development.JSON* pliku katalog główny projektu jest `IdentityServer` sekcji, który opisuje klucz używany do podpisywania tokenów.</span><span class="sxs-lookup"><span data-stu-id="c859d-153">In the *appsettings.Development.json* file of the project root, there's an `IdentityServer` section that describes the key used to sign tokens.</span></span> <span data-ttu-id="c859d-154">Podczas wdrażania do środowiska produkcyjnego, klucz musi zostać aprowizowane i wdrażana wraz z aplikacji, jak wyjaśniono w [Wdróż do produkcji](#deploy-to-production) sekcji.</span><span class="sxs-lookup"><span data-stu-id="c859d-154">When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section.</span></span>

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a><span data-ttu-id="c859d-155">Ogólny opis aplikacji Angular</span><span class="sxs-lookup"><span data-stu-id="c859d-155">General description of the Angular app</span></span>

<span data-ttu-id="c859d-156">Uwierzytelnianie i autoryzacja interfejs API obsługuje w Angular szablon znajduje się w jego własnej moduł Angular w *ClientApp\src\api autoryzacji* katalogu.</span><span class="sxs-lookup"><span data-stu-id="c859d-156">The authentication and API authorization support in the Angular template resides in its own Angular module in the *ClientApp\src\api-authorization* directory.</span></span> <span data-ttu-id="c859d-157">Moduł składa się z następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="c859d-157">The module is composed of the following elements:</span></span>

* <span data-ttu-id="c859d-158">składniki 3:</span><span class="sxs-lookup"><span data-stu-id="c859d-158">3 components:</span></span>
  * <span data-ttu-id="c859d-159">*Login.Component.TS*: Obsługuje przepływu logowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-159">*login.component.ts*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="c859d-160">*logout.Component.TS*: Obsługuje przepływu wylogowania z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-160">*logout.component.ts*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="c859d-161">*Zaloguj się menu.component.ts*: Widżet, który wyświetli jeden z następujących linków:</span><span class="sxs-lookup"><span data-stu-id="c859d-161">*login-menu.component.ts*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="c859d-162">Zarządzanie profilami użytkowników i łącza, gdy użytkownik jest uwierzytelniany w przypadku wylogowania.</span><span class="sxs-lookup"><span data-stu-id="c859d-162">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="c859d-163">Rejestracja i logowanie łącza, gdy użytkownik nie jest uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="c859d-163">Registration and log in links when the user isn't authenticated.</span></span>
* <span data-ttu-id="c859d-164">Strażnik trasy `AuthorizeGuard` , mogą być dodawane do trasy i wymaga od użytkownika uwierzytelniali się przed odwiedzający trasy.</span><span class="sxs-lookup"><span data-stu-id="c859d-164">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="c859d-165">Interceptor HTTP `AuthorizeInterceptor` , dołącza token dostępu na wychodzące żądania HTTP, przeznaczone dla interfejsu API, gdy użytkownik jest uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="c859d-165">An HTTP interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="c859d-166">Usługa `AuthorizeService` , obsługuje szczegóły niższego poziomu procesu uwierzytelniania i udostępnia informacje o uwierzytelnionym użytkowniku w pozostałej części aplikacji do użycia.</span><span class="sxs-lookup"><span data-stu-id="c859d-166">A service `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>
* <span data-ttu-id="c859d-167">Moduł Angular, definiujący tras skojarzone z uwierzytelniania części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-167">An Angular module that defines routes associated with the authentication parts of the app.</span></span> <span data-ttu-id="c859d-168">Udostępnia ona składnika menu logowania, interceptor, osłony i usługi do użycia od pozostałej części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-168">It exposes the login menu component, the interceptor, the guard, and the service for consumption from the rest of the app.</span></span>

## <a name="general-description-of-the-react-app"></a><span data-ttu-id="c859d-169">Ogólny opis aplikacji platformy React</span><span class="sxs-lookup"><span data-stu-id="c859d-169">General description of the React app</span></span>

<span data-ttu-id="c859d-170">Obsługa uwierzytelniania i autoryzacji interfejsu API w szablonie platformy React znajduje się w *ClientApp\src\components\api autoryzacji* katalogu.</span><span class="sxs-lookup"><span data-stu-id="c859d-170">The support for authentication and API authorization in the React template resides in the *ClientApp\src\components\api-authorization* directory.</span></span> <span data-ttu-id="c859d-171">Składa się z następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="c859d-171">It's composed of the following elements:</span></span>

* <span data-ttu-id="c859d-172">4 składniki:</span><span class="sxs-lookup"><span data-stu-id="c859d-172">4 components:</span></span>
  * <span data-ttu-id="c859d-173">*Login.js*: Obsługuje przepływu logowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-173">*Login.js*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="c859d-174">*Logout.js*: Obsługuje przepływu wylogowania z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-174">*Logout.js*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="c859d-175">*LoginMenu.js*: Widżet, który wyświetli jeden z następujących linków:</span><span class="sxs-lookup"><span data-stu-id="c859d-175">*LoginMenu.js*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="c859d-176">Zarządzanie profilami użytkowników i łącza, gdy użytkownik jest uwierzytelniany w przypadku wylogowania.</span><span class="sxs-lookup"><span data-stu-id="c859d-176">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="c859d-177">Rejestracja i logowanie łącza, gdy użytkownik nie jest uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="c859d-177">Registration and log in links when the user isn't authenticated.</span></span>
  * <span data-ttu-id="c859d-178">*AuthorizeRoute.js*: Składnik trasy, który wymaga od użytkownika uwierzytelniali się przed renderowaniem składnika czcionką `Component` parametru.</span><span class="sxs-lookup"><span data-stu-id="c859d-178">*AuthorizeRoute.js*: A route component that requires a user to be authenticated before rendering the component indicated in the `Component` parameter.</span></span>
* <span data-ttu-id="c859d-179">Wyeksportowane `authService` wystąpienia klasy `AuthorizeService` , obsługuje szczegóły niższego poziomu procesu uwierzytelniania i udostępnia informacje o uwierzytelnionym użytkowniku w pozostałej części aplikacji do użycia.</span><span class="sxs-lookup"><span data-stu-id="c859d-179">An exported `authService` instance of class `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>

<span data-ttu-id="c859d-180">Skoro wiesz głównymi składnikami rozwiązania może potrwać lepiej poznać poszczególne scenariusze dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-180">Now that you've seen the main components of the solution, you can take a deeper look at individual scenarios for the app.</span></span>

## <a name="require-authorization-on-a-new-api"></a><span data-ttu-id="c859d-181">Wymaga zezwolenia na nowy interfejs API</span><span class="sxs-lookup"><span data-stu-id="c859d-181">Require authorization on a new API</span></span>

<span data-ttu-id="c859d-182">Domyślnie system jest skonfigurowany do łatwego wymagają autoryzacji dla nowych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="c859d-182">By default, the system is configured to easily require authorization for new APIs.</span></span> <span data-ttu-id="c859d-183">Aby to zrobić, Utwórz nowy kontroler i Dodaj `[Authorize]` atrybutów do klasy kontrolera lub dowolnych akcji w obrębie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c859d-183">To do so, create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protect-a-client-side-route-angular"></a><span data-ttu-id="c859d-184">Ochrona trasę po stronie klienta (Angular)</span><span class="sxs-lookup"><span data-stu-id="c859d-184">Protect a client-side route (Angular)</span></span>

<span data-ttu-id="c859d-185">Ochrona trasy klienta odbywa się przez dodanie guard autoryzacji do listy osłony do uruchomienia podczas konfigurowania trasy.</span><span class="sxs-lookup"><span data-stu-id="c859d-185">Protecting a client-side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="c859d-186">Na przykład możesz zobaczyć jak `fetch-data` trasa jest skonfigurowana w module usługi Angular głównej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c859d-186">As an example, you can see how the `fetch-data` route is configured within the main app Angular module:</span></span>

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="c859d-187">Ważne jest, aby wspomnieć, że ochrona trasy nie chroni istniejący punkt końcowy (które nadal wymaga `[Authorize]` zastosować atrybut), ale czy tylko uniemożliwia użytkownikowi przechodzić do danej trasy po stronie klienta, gdy go nie jest uwierzytelniany na.</span><span class="sxs-lookup"><span data-stu-id="c859d-187">It's important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="c859d-188">Uwierzytelnianie żądań interfejsu API (Angular)</span><span class="sxs-lookup"><span data-stu-id="c859d-188">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="c859d-189">Uwierzytelniania żądań do interfejsów API hostowanych obok aplikacji odbywa się automatycznie przy użyciu interceptor klienta HTTP, które są zdefiniowane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="c859d-189">Authenticating requests to APIs hosted alongside the app is done automatically through the use of the HTTP client interceptor defined by the app.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="c859d-190">Ochrona trasę po stronie klienta (React)</span><span class="sxs-lookup"><span data-stu-id="c859d-190">Protect a client-side route (React)</span></span>

<span data-ttu-id="c859d-191">Ochrona trasy po stronie klienta za pomocą `AuthorizeRoute` składnika zamiast zwykłego `Route` składnika.</span><span class="sxs-lookup"><span data-stu-id="c859d-191">Protect a client-side route by using the `AuthorizeRoute` component instead of the plain `Route` component.</span></span> <span data-ttu-id="c859d-192">Na przykład, zwróć uwagę, jak `fetch-data` trasa jest skonfigurowana w ramach `App` składników:</span><span class="sxs-lookup"><span data-stu-id="c859d-192">For example, notice how the `fetch-data` route is configured within the `App` component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="c859d-193">Ochrona trasy:</span><span class="sxs-lookup"><span data-stu-id="c859d-193">Protecting a route:</span></span>

* <span data-ttu-id="c859d-194">Nie chroni istniejący punkt końcowy (które nadal wymaga `[Authorize]` zastosowany do niego).</span><span class="sxs-lookup"><span data-stu-id="c859d-194">Doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it).</span></span>
* <span data-ttu-id="c859d-195">Zapobiega tylko użytkownika przejdź do daną trasą po stronie klienta, gdy go nie jest uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="c859d-195">Only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="c859d-196">Uwierzytelnianie żądań interfejsu API (React)</span><span class="sxs-lookup"><span data-stu-id="c859d-196">Authenticate API requests (React)</span></span>

<span data-ttu-id="c859d-197">Uwierzytelnianie żądań przy użyciu platformy React odbywa się przez importowanie pierwszego `authService` wystąpienia z `AuthorizeService`.</span><span class="sxs-lookup"><span data-stu-id="c859d-197">Authenticating requests with React is done by first importing the `authService` instance from the `AuthorizeService`.</span></span> <span data-ttu-id="c859d-198">Token dostępu jest pobierana z `authService` i jest dołączony do żądania, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="c859d-198">The access token is retrieved from the `authService` and is attached to the request as shown below.</span></span> <span data-ttu-id="c859d-199">Składniki platformy React, ta praca odbywa się zwykle `componentDidMount` metoda cyklu życia lub w wyniku interakcji z użytkownikiem.</span><span class="sxs-lookup"><span data-stu-id="c859d-199">In React components, this work is typically done in the `componentDidMount` lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="c859d-200">Importowanie authService do składnika</span><span class="sxs-lookup"><span data-stu-id="c859d-200">Import the authService into your component</span></span>

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="c859d-201">Pobieranie i dołączanie token dostępu do odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="c859d-201">Retrieve and attach the access token to the response</span></span>

```javascript
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-to-production"></a><span data-ttu-id="c859d-202">Wdrażanie w środowisku produkcyjnym</span><span class="sxs-lookup"><span data-stu-id="c859d-202">Deploy to production</span></span>

<span data-ttu-id="c859d-203">Aby wdrożyć aplikację do środowiska produkcyjnego, należy aprowizować następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="c859d-203">To deploy the app to production, the following resources need to be provisioned:</span></span>

* <span data-ttu-id="c859d-204">Bazę danych do przechowywania tożsamości konta użytkowników i przyznaje IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="c859d-204">A database to store the Identity user accounts and the IdentityServer grants.</span></span>
* <span data-ttu-id="c859d-205">Certyfikatu produkcyjnego do użycia podczas podpisywania tokenów.</span><span class="sxs-lookup"><span data-stu-id="c859d-205">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="c859d-206">Nie istnieją określone wymagania dotyczące tego certyfikatu; może być certyfikat z podpisem własnym lub certyfikatu dostarczanymi za pośrednictwem urzędu certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-206">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="c859d-207">Mogą być generowane przy użyciu standardowych narzędzi, takich jak program PowerShell lub protokołu OpenSSL.</span><span class="sxs-lookup"><span data-stu-id="c859d-207">It can be generated through standard tools like PowerShell or OpenSSL.</span></span>
  * <span data-ttu-id="c859d-208">Mogą być zainstalowane w magazynie certyfikatów na komputerach docelowych lub wdrożony jako *PFX* pliku silnym hasłem.</span><span class="sxs-lookup"><span data-stu-id="c859d-208">It can be installed into the certificate store on the target machines or deployed as a *.pfx* file with a strong password.</span></span>

### <a name="example-deploy-to-azure-websites"></a><span data-ttu-id="c859d-209">Przykład: Wdrażanie w usłudze Azure Websites</span><span class="sxs-lookup"><span data-stu-id="c859d-209">Example: Deploy to Azure Websites</span></span>

<span data-ttu-id="c859d-210">W tej sekcji opisano wdrażanie aplikacji w usłudze Azure websites przy użyciu certyfikatu przechowywanego w magazynie certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="c859d-210">This section describes deploying the app to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="c859d-211">Aby zmodyfikować aplikację, aby załadować certyfikatu z magazynu certyfikatów, plan usługi App Service musi być włączona co najmniej warstwę standardowa podczas konfigurowania w późniejszym kroku.</span><span class="sxs-lookup"><span data-stu-id="c859d-211">To modify the app to load a certificate from the certificate store, the App Service plan needs to be on at least the Standard tier when you configure in a later step.</span></span> <span data-ttu-id="c859d-212">W aplikacji *appsettings.json* pliku, zmodyfikuj `IdentityServer` sekcji, aby dołączyć szczegóły klucza:</span><span class="sxs-lookup"><span data-stu-id="c859d-212">In the app's *appsettings.json* file, modify the `IdentityServer` section to include the key details:</span></span>

```json
"IdentityServer": {
  "Key": {
    "Type": "Store",
    "StoreName": "My",
    "StoreLocation": "CurrentUser",
    "Name": "CN=MyApplication"
  }
}
```

* <span data-ttu-id="c859d-213">Właściwość Nazwa certyfikatu odpowiada wyróżniająca podmiotu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="c859d-213">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="c859d-214">Lokalizacja magazynu reprezentuje, gdzie można załadować certyfikatu z (`CurrentUser` lub `LocalMachine`).</span><span class="sxs-lookup"><span data-stu-id="c859d-214">The store location represents where to load the certificate from (`CurrentUser` or `LocalMachine`).</span></span>
* <span data-ttu-id="c859d-215">Nazwa magazynu reprezentuje nazwę magazynu certyfikatów, gdy certyfikat jest przechowywany.</span><span class="sxs-lookup"><span data-stu-id="c859d-215">The store name represents the name of the certificate store where the certificate is stored.</span></span> <span data-ttu-id="c859d-216">W tym przypadku punktów w magazynie osobistym użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c859d-216">In this case, it points to the personal user store.</span></span>

<span data-ttu-id="c859d-217">Aby wdrożyć w usłudze Azure Websites, wdrażanie aplikacji, wykonaj czynności w [wdrażanie aplikacji na platformie Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) zasobów platformy Azure niezbędnych do tworzenia i wdrażania aplikacji w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="c859d-217">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="c859d-218">Po wykonaniu poprzednich instrukcji, aplikacja jest wdrażana na platformie Azure, ale nie jest jeszcze funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="c859d-218">After following the preceding instructions, the app is deployed to Azure but isn't yet functional.</span></span> <span data-ttu-id="c859d-219">Certyfikat używany przez aplikację, która jest nadal potrzebuje do skonfigurowania.</span><span class="sxs-lookup"><span data-stu-id="c859d-219">The certificate used by the app still needs to be set up.</span></span> <span data-ttu-id="c859d-220">Znajdź odcisk palca certyfikatu, który ma być używany, a następnie wykonaj kroki opisane w [ładowanie certyfikatów](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span><span class="sxs-lookup"><span data-stu-id="c859d-220">Locate the thumbprint for the certificate to be used, and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span></span>

<span data-ttu-id="c859d-221">Podczas tych czynności wspomina o identyfikatorach protokołu SSL znajduje się **certyfikaty prywatne** sekcji w portalu, gdzie możesz przekazać certyfikat aprowizowane za pomocą aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-221">While these steps mention SSL, there's a **Private certificates** section on the portal where you can upload the provisioned certificate to use with the app.</span></span>

<span data-ttu-id="c859d-222">Po wykonaniu tego kroku, uruchom ponownie aplikację i powinna być funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="c859d-222">After this step, restart the app and it should be functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="c859d-223">Inne opcje konfiguracji</span><span class="sxs-lookup"><span data-stu-id="c859d-223">Other configuration options</span></span>

<span data-ttu-id="c859d-224">Obsługa interfejsu API autoryzacji opartą na IdentityServer z zestawem Konwencji, wartości domyślne i ulepszenia, aby uprościć środowisko dla aplikacji jednostronicowych.</span><span class="sxs-lookup"><span data-stu-id="c859d-224">The support for API authorization builds on top of IdentityServer with a set of conventions, default values, and enhancements to simplify the experience for SPAs.</span></span> <span data-ttu-id="c859d-225">Needless, że pełnych możliwości IdentityServer jest dostępna w tle integracji platformy ASP.NET Core nie obejmuje Twojego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="c859d-225">Needless to say, the full power of IdentityServer is available behind the scenes if the ASP.NET Core integrations don't cover your scenario.</span></span> <span data-ttu-id="c859d-226">Obsługa platformy ASP.NET Core koncentruje się na "" aplikacje firmy, gdzie wszystkie aplikacje są tworzone i wdrażane przez naszej organizacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-226">The ASP.NET Core support is focused on "first-party" apps, where all the apps are created and deployed by our organization.</span></span> <span data-ttu-id="c859d-227">Jako takie pomoc techniczna nie jest oferowana w przypadku elementów, takich jak zgody lub federacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-227">As such, support isn't offered for things like consent or federation.</span></span> <span data-ttu-id="c859d-228">W tych scenariuszach Użyj IdentityServer i postępuj zgodnie z ich dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-228">For those scenarios, use IdentityServer and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="c859d-229">Profile aplikacji</span><span class="sxs-lookup"><span data-stu-id="c859d-229">Application profiles</span></span>

<span data-ttu-id="c859d-230">Profile aplikacji są wstępnie zdefiniowane konfiguracje dla aplikacji, które uściślić swoich parametrów.</span><span class="sxs-lookup"><span data-stu-id="c859d-230">Application profiles are predefined configurations for apps that further define their parameters.</span></span> <span data-ttu-id="c859d-231">W tej chwili obsługiwane są następujące profile:</span><span class="sxs-lookup"><span data-stu-id="c859d-231">At this time, the following profiles are supported:</span></span>

* <span data-ttu-id="c859d-232">`IdentityServerSPA`: Reprezentuje SPA, hostowane obok IdentityServer jako pojedyncza jednostka.</span><span class="sxs-lookup"><span data-stu-id="c859d-232">`IdentityServerSPA`: Represents a SPA hosted alongside IdentityServer as a single unit.</span></span>
  * <span data-ttu-id="c859d-233">`redirect_uri` Wartość domyślna to `/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="c859d-233">The `redirect_uri` defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="c859d-234">`post_logout_redirect_uri` Wartość domyślna to `/authentication/logout-callback`.</span><span class="sxs-lookup"><span data-stu-id="c859d-234">The `post_logout_redirect_uri` defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="c859d-235">Obejmuje zestaw zakresów `openid`, `profile`i każdy zakres zdefiniowany dla interfejsów API w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-235">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="c859d-236">Zestaw dozwolonych typów odpowiedzi OIDC jest `id_token token` lub każdego z nich osobno (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="c859d-236">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="c859d-237">Tryb odpowiedzi dozwolone jest `fragment`.</span><span class="sxs-lookup"><span data-stu-id="c859d-237">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="c859d-238">`SPA`: Reprezentuje SPA, która nie jest obsługiwana przy użyciu IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="c859d-238">`SPA`: Represents a SPA that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="c859d-239">Obejmuje zestaw zakresów `openid`, `profile`i każdy zakres zdefiniowany dla interfejsów API w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-239">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="c859d-240">Zestaw dozwolonych typów odpowiedzi OIDC jest `id_token token` lub każdego z nich osobno (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="c859d-240">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="c859d-241">Tryb odpowiedzi dozwolone jest `fragment`.</span><span class="sxs-lookup"><span data-stu-id="c859d-241">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="c859d-242">`IdentityServerJwt`: Reprezentuje interfejs API, który znajduje się wraz z IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="c859d-242">`IdentityServerJwt`: Represents an API that is hosted alongside with IdentityServer.</span></span>
  * <span data-ttu-id="c859d-243">Aplikacja jest skonfigurowana z jednego zakresu, które domyślnie jest to nazwa aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-243">The app is configured to have a single scope that defaults to the app name.</span></span>
* <span data-ttu-id="c859d-244">`API`: Reprezentuje interfejs API, która nie jest obsługiwana z IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="c859d-244">`API`: Represents an API that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="c859d-245">Aplikacja jest skonfigurowana z jednego zakresu, które domyślnie jest to nazwa aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c859d-245">The app is configured to have a single scope that defaults to the app name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="c859d-246">Konfigurowanie za pomocą AppSettings</span><span class="sxs-lookup"><span data-stu-id="c859d-246">Configuration through AppSettings</span></span>

<span data-ttu-id="c859d-247">Konfigurowanie aplikacji przy użyciu systemu konfiguracji, dodając je do listy `Clients` lub `Resources`.</span><span class="sxs-lookup"><span data-stu-id="c859d-247">Configure the apps through the configuration system by adding them to the list of `Clients` or `Resources`.</span></span>

<span data-ttu-id="c859d-248">Skonfiguruj każdy klient `redirect_uri` i `post_logout_redirect_uri` właściwości, jak pokazano w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c859d-248">Configure each client's `redirect_uri` and `post_logout_redirect_uri` property, as shown in the following example:</span></span>

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

<span data-ttu-id="c859d-249">Podczas konfigurowania zasobów, można skonfigurować zakresy dla zasobu, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="c859d-249">When configuring resources, you can configure the scopes for the resource as shown below:</span></span>

```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c"
    }
  }
}
```

### <a name="configuration-through-code"></a><span data-ttu-id="c859d-250">Konfigurowanie za pomocą kodu</span><span class="sxs-lookup"><span data-stu-id="c859d-250">Configuration through code</span></span>

<span data-ttu-id="c859d-251">Można również skonfigurować klientów i zasobów za pomocą kodu za pomocą przeciążenia `AddApiAuthorization` , wykonują akcję, aby skonfigurować opcje.</span><span class="sxs-lookup"><span data-stu-id="c859d-251">You can also configure the clients and resources through code using an overload of `AddApiAuthorization` that takes an action to configure options.</span></span>

```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA", spa =>
        spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
           .WithLogoutRedirectUri(
               "http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource =>
        resource.WithScopes("a", "b", "c"));
});
```

## <a name="additional-resources"></a><span data-ttu-id="c859d-252">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c859d-252">Additional resources</span></span>

* <xref:spa/angular>
* <xref:spa/react>
