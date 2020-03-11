---
title: Wprowadzenie do uwierzytelniania aplikacji jednostronicowych na ASP.NET Core
author: javiercn
description: Używanie tożsamości z aplikacją jednostronicową hostowaną w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 11/08/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 623f739b17c0bed3ce929f562c9581ab26ecf5bc
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661414"
---
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="c16cf-103">Uwierzytelnianie i autoryzacja dla aplikacji jednostronicowych</span><span class="sxs-lookup"><span data-stu-id="c16cf-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="c16cf-104">ASP.NET Core 3,0 lub nowszy oferuje uwierzytelnianie w aplikacjach jednostronicowych (aplikacji jednostronicowych) przy użyciu obsługi autoryzacji interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="c16cf-104">ASP.NET Core 3.0 or later offers authentication in Single Page Apps (SPAs) using the support for API authorization.</span></span> <span data-ttu-id="c16cf-105">ASP.NET Core tożsamość uwierzytelniania i przechowywania użytkowników jest połączona z [IdentityServer](https://identityserver.io/) w celu zaimplementowania połączenia Open ID Connect.</span><span class="sxs-lookup"><span data-stu-id="c16cf-105">ASP.NET Core Identity for authenticating and storing users is combined with [IdentityServer](https://identityserver.io/) for implementing Open ID Connect.</span></span>

<span data-ttu-id="c16cf-106">Parametr uwierzytelniania został dodany do szablonów projektów **kątowych** i **reagowania** , które są podobne do parametrów uwierzytelniania w szablonach projektu **aplikacji sieci Web (Model-View-Controller)** (MVC) i **aplikacji sieci Web** (Razor Pages).</span><span class="sxs-lookup"><span data-stu-id="c16cf-106">An authentication parameter was added to the **Angular** and **React** project templates that is similar to the authentication parameter in the **Web Application (Model-View-Controller)** (MVC) and **Web Application** (Razor Pages) project templates.</span></span> <span data-ttu-id="c16cf-107">Dozwolone wartości parametrów to **none** i **indywidualny**.</span><span class="sxs-lookup"><span data-stu-id="c16cf-107">The allowed parameter values are **None** and **Individual**.</span></span> <span data-ttu-id="c16cf-108">Szablon projektu **re. js i Redux** nie obsługuje teraz parametru Authentication.</span><span class="sxs-lookup"><span data-stu-id="c16cf-108">The **React.js and Redux** project template doesn't support the authentication parameter at this time.</span></span>

## <a name="create-an-app-with-api-authorization-support"></a><span data-ttu-id="c16cf-109">Tworzenie aplikacji z obsługą autoryzacji interfejsu API</span><span class="sxs-lookup"><span data-stu-id="c16cf-109">Create an app with API authorization support</span></span>

<span data-ttu-id="c16cf-110">Uwierzytelnianie i autoryzacja użytkowników mogą być używane z aplikacji jednostronicowychą kątową i reagują.</span><span class="sxs-lookup"><span data-stu-id="c16cf-110">User authentication and authorization can be used with both Angular and React SPAs.</span></span> <span data-ttu-id="c16cf-111">Otwórz powłokę wiersza polecenia, a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="c16cf-111">Open a command shell, and run the following command:</span></span>

<span data-ttu-id="c16cf-112">**Kątowy**:</span><span class="sxs-lookup"><span data-stu-id="c16cf-112">**Angular**:</span></span>

```dotnetcli
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="c16cf-113">**Reagowanie**:</span><span class="sxs-lookup"><span data-stu-id="c16cf-113">**React**:</span></span>

```dotnetcli
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="c16cf-114">Poprzednie polecenie tworzy aplikację ASP.NET Core z katalogiem *ClientApp* zawierającym spa.</span><span class="sxs-lookup"><span data-stu-id="c16cf-114">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the SPA.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="c16cf-115">Ogólny opis składników ASP.NET Core aplikacji</span><span class="sxs-lookup"><span data-stu-id="c16cf-115">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="c16cf-116">W poniższych sekcjach opisano Dodatki do projektu w przypadku włączenia obsługi uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="c16cf-116">The following sections describe additions to the project when authentication support is included:</span></span>

### <a name="startup-class"></a><span data-ttu-id="c16cf-117">Klasa początkowa</span><span class="sxs-lookup"><span data-stu-id="c16cf-117">Startup class</span></span>

<span data-ttu-id="c16cf-118">Klasa `Startup` ma następujące dodatki:</span><span class="sxs-lookup"><span data-stu-id="c16cf-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="c16cf-119">Wewnątrz metody `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c16cf-119">Inside the `Startup.ConfigureServices` method:</span></span>
  * <span data-ttu-id="c16cf-120">Tożsamość z domyślnym interfejsem użytkownika:</span><span class="sxs-lookup"><span data-stu-id="c16cf-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="c16cf-121">IdentityServer z dodatkową metodą pomocniczą `AddApiAuthorization`, która konfiguruje niektóre domyślne konwencje ASP.NET Core w oparciu o IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="c16cf-121">IdentityServer with an additional `AddApiAuthorization` helper method that sets up some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="c16cf-122">Uwierzytelnianie za pomocą dodatkowej metody pomocnika `AddIdentityServerJwt`, która konfiguruje aplikację do weryfikowania tokenów JWT utworzonych przez IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="c16cf-122">Authentication with an additional `AddIdentityServerJwt` helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="c16cf-123">Wewnątrz metody `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="c16cf-123">Inside the `Startup.Configure` method:</span></span>
  * <span data-ttu-id="c16cf-124">Oprogramowanie pośredniczące uwierzytelniania odpowiedzialne za Weryfikowanie poświadczeń żądania i Ustawianie użytkownika w kontekście żądania:</span><span class="sxs-lookup"><span data-stu-id="c16cf-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="c16cf-125">Oprogramowanie pośredniczące IdentityServer, które uwidacznia punkty końcowe połączenia Open ID:</span><span class="sxs-lookup"><span data-stu-id="c16cf-125">The IdentityServer middleware that exposes the Open ID Connect endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="c16cf-126">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="c16cf-126">AddApiAuthorization</span></span>

<span data-ttu-id="c16cf-127">Ta metoda pomocnika umożliwia skonfigurowanie IdentityServer do korzystania z naszej obsługiwanej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-127">This helper method configures IdentityServer to use our supported configuration.</span></span> <span data-ttu-id="c16cf-128">IdentityServer to zaawansowane i rozszerzalne środowisko do obsługi zagadnień związanych z zabezpieczeniami aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="c16cf-129">W tym samym czasie, które ujawnia niezbędną złożoność dla najbardziej typowych scenariuszy.</span><span class="sxs-lookup"><span data-stu-id="c16cf-129">At the same time, that exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="c16cf-130">W związku z tym zestaw Konwencji i opcji konfiguracji jest dostarczany do użytkownika, który jest uważany za dobry punkt wyjścia.</span><span class="sxs-lookup"><span data-stu-id="c16cf-130">Consequently, a set of conventions and configuration options is provided to you that are considered a good starting point.</span></span> <span data-ttu-id="c16cf-131">Gdy uwierzytelnianie będzie wymagało zmiany, pełne możliwości IdentityServer są nadal dostępne, aby dostosować uwierzytelnianie zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="c16cf-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="c16cf-132">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="c16cf-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="c16cf-133">Ta metoda pomocnika konfiguruje schemat zasad dla aplikacji jako domyślną procedurę obsługi uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="c16cf-133">This helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="c16cf-134">Zasady są skonfigurowane tak, aby umożliwić obsługę tożsamości wszystkie żądania kierowane do dowolnej ścieżki podrzędnej w adresie URL tożsamości "/Identity".</span><span class="sxs-lookup"><span data-stu-id="c16cf-134">The policy is configured to let Identity handle all requests routed to any subpath in the Identity URL space "/Identity".</span></span> <span data-ttu-id="c16cf-135">`JwtBearerHandler` obsługuje wszystkie inne żądania.</span><span class="sxs-lookup"><span data-stu-id="c16cf-135">The `JwtBearerHandler` handles all other requests.</span></span> <span data-ttu-id="c16cf-136">Ponadto ta metoda rejestruje zasób interfejsu API `<<ApplicationName>>API` przy użyciu usługi IdentityServer z domyślnym zakresem `<<ApplicationName>>API` i konfiguruje oprogramowanie pośredniczące tokenu okaziciela JWT do weryfikowania tokenów wystawionych przez IdentityServer dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-136">Additionally, this method registers an `<<ApplicationName>>API` API resource with IdentityServer with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="weatherforecastcontroller"></a><span data-ttu-id="c16cf-137">WeatherForecastController</span><span class="sxs-lookup"><span data-stu-id="c16cf-137">WeatherForecastController</span></span>

<span data-ttu-id="c16cf-138">W pliku *Controllers\WeatherForecastController.cs* Zwróć uwagę na atrybut `[Authorize]` stosowany do klasy, która wskazuje, że użytkownik musi być autoryzowany na podstawie domyślnych zasad dostępu do zasobu.</span><span class="sxs-lookup"><span data-stu-id="c16cf-138">In the *Controllers\WeatherForecastController.cs* file, notice the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="c16cf-139">Domyślne zasady autoryzacji są skonfigurowane tak, aby korzystały z domyślnego schematu uwierzytelniania, który jest konfigurowany przez `AddIdentityServerJwt` do schematu zasad, który został wymieniony powyżej, a `JwtBearerHandler` skonfigurowany przez taką metodę pomocnika domyślną procedurę obsługi dla żądań do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-139">The default authorization policy happens to be configured to use the default authentication scheme, which is set up by `AddIdentityServerJwt` to the policy scheme that was mentioned above, making the `JwtBearerHandler` configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="c16cf-140">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="c16cf-140">ApplicationDbContext</span></span>

<span data-ttu-id="c16cf-141">W pliku *Data\ApplicationDbContext.cs* należy zauważyć, że ten sam `DbContext` jest używany w tożsamości z wyjątkiem, że rozszerza `ApiAuthorizationDbContext` (bardziej pochodna klasa z `IdentityDbContext`), aby uwzględnić schemat dla IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="c16cf-141">In the *Data\ApplicationDbContext.cs* file, notice the same `DbContext` is used in Identity with the exception that it extends `ApiAuthorizationDbContext` (a more derived class from `IdentityDbContext`) to include the schema for IdentityServer.</span></span>

<span data-ttu-id="c16cf-142">Aby uzyskać pełną kontrolę nad schematem bazy danych, należy podziedziczyć z jednej z dostępnych `DbContext` klas i skonfigurować kontekst w celu uwzględnienia schematu tożsamości, wywołując `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` na metodę `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="c16cf-142">To gain full control of the database schema, inherit from one of the available Identity `DbContext` classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="c16cf-143">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="c16cf-143">OidcConfigurationController</span></span>

<span data-ttu-id="c16cf-144">W pliku *Controllers\OidcConfigurationController.cs* Zwróć uwagę na punkt końcowy, który jest wstępnie zainicjowany do obsługi parametrów OIDC wymaganych przez klienta.</span><span class="sxs-lookup"><span data-stu-id="c16cf-144">In the *Controllers\OidcConfigurationController.cs* file, notice the endpoint that's provisioned to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="c16cf-145">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="c16cf-145">appsettings.json</span></span>

<span data-ttu-id="c16cf-146">W pliku *appSettings. JSON* w katalogu głównym projektu znajduje się nowa sekcja `IdentityServer` opisująca listę skonfigurowanych klientów.</span><span class="sxs-lookup"><span data-stu-id="c16cf-146">In the *appsettings.json* file of the project root, there's a new `IdentityServer` section that describes the list of configured clients.</span></span> <span data-ttu-id="c16cf-147">W poniższym przykładzie istnieje pojedynczy klient.</span><span class="sxs-lookup"><span data-stu-id="c16cf-147">In the following example, there's a single client.</span></span> <span data-ttu-id="c16cf-148">Nazwa klienta odpowiada nazwie aplikacji i jest zamapowana według Konwencji do parametru `ClientId` protokołu OAuth.</span><span class="sxs-lookup"><span data-stu-id="c16cf-148">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="c16cf-149">Profil wskazuje konfigurowany typ aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-149">The profile indicates the app type being configured.</span></span> <span data-ttu-id="c16cf-150">Jest on używany wewnętrznie w przypadku Konwencji, które upraszczają proces konfiguracji serwera.</span><span class="sxs-lookup"><span data-stu-id="c16cf-150">It's used internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="c16cf-151">Istnieje kilka dostępnych profilów, zgodnie z opisem w sekcji [Profile aplikacji](#application-profiles) .</span><span class="sxs-lookup"><span data-stu-id="c16cf-151">There are several profiles available, as explained in the [Application profiles](#application-profiles) section.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="c16cf-152">appsettings.Development.json</span><span class="sxs-lookup"><span data-stu-id="c16cf-152">appsettings.Development.json</span></span>

<span data-ttu-id="c16cf-153">W pliku *appSettings. Plik Development. JSON* w katalogu głównym projektu zawiera sekcję `IdentityServer` opisującą klucz używany do podpisywania tokenów.</span><span class="sxs-lookup"><span data-stu-id="c16cf-153">In the *appsettings.Development.json* file of the project root, there's an `IdentityServer` section that describes the key used to sign tokens.</span></span> <span data-ttu-id="c16cf-154">Podczas wdrażania w środowisku produkcyjnym należy zainicjować i wdrożyć klucz wraz z aplikacją, jak wyjaśniono w sekcji [wdrażanie w środowisku produkcyjnym](#deploy-to-production) .</span><span class="sxs-lookup"><span data-stu-id="c16cf-154">When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section.</span></span>

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a><span data-ttu-id="c16cf-155">Ogólny opis aplikacji kątowej</span><span class="sxs-lookup"><span data-stu-id="c16cf-155">General description of the Angular app</span></span>

<span data-ttu-id="c16cf-156">Obsługa uwierzytelniania i autoryzacji interfejsu API w szablonie kątowym znajduje się w jego własnym module skośnym w katalogu *ClientApp\src\api-Authorization* .</span><span class="sxs-lookup"><span data-stu-id="c16cf-156">The authentication and API authorization support in the Angular template resides in its own Angular module in the *ClientApp\src\api-authorization* directory.</span></span> <span data-ttu-id="c16cf-157">Moduł składa się z następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="c16cf-157">The module is composed of the following elements:</span></span>

* <span data-ttu-id="c16cf-158">3 składniki:</span><span class="sxs-lookup"><span data-stu-id="c16cf-158">3 components:</span></span>
  * <span data-ttu-id="c16cf-159">*login. Component. TS*: obsługuje przepływ logowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-159">*login.component.ts*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="c16cf-160">*Wyloguj. składnik. TS*: obsługuje przepływ wylogowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-160">*logout.component.ts*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="c16cf-161">*login-menu. składnik. TS*: element widget wyświetlający jeden z następujących zestawów linków:</span><span class="sxs-lookup"><span data-stu-id="c16cf-161">*login-menu.component.ts*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="c16cf-162">Zarządzanie profilami użytkowników i wylogowywanie łączy podczas uwierzytelniania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c16cf-162">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="c16cf-163">Rejestrowanie i logowanie w przypadku braku uwierzytelnienia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c16cf-163">Registration and log in links when the user isn't authenticated.</span></span>
* <span data-ttu-id="c16cf-164">`AuthorizeGuard` ochrony trasy, którą można dodać do tras i wymaga uwierzytelnienia użytkownika przed odwiedzeniem trasy.</span><span class="sxs-lookup"><span data-stu-id="c16cf-164">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="c16cf-165">`AuthorizeInterceptor` Interceptor protokołu HTTP, który dołącza token dostępu do wychodzących żądań HTTP przeznaczonych dla interfejsu API w przypadku uwierzytelnienia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c16cf-165">An HTTP interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="c16cf-166">`AuthorizeService` usługi, która obsługuje szczegóły niższego poziomu procesu uwierzytelniania i ujawnia informacje o uwierzytelnionym użytkowniku w pozostałej części aplikacji do użycia.</span><span class="sxs-lookup"><span data-stu-id="c16cf-166">A service `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>
* <span data-ttu-id="c16cf-167">Moduł kątowy, który definiuje trasy skojarzone z częściami uwierzytelniania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-167">An Angular module that defines routes associated with the authentication parts of the app.</span></span> <span data-ttu-id="c16cf-168">Przedstawia on składnik menu logowania, Interceptor, ochronę i usługę do użycia w pozostałej części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-168">It exposes the login menu component, the interceptor, the guard, and the service for consumption from the rest of the app.</span></span>

## <a name="general-description-of-the-react-app"></a><span data-ttu-id="c16cf-169">Ogólny opis aplikacji do reagowania</span><span class="sxs-lookup"><span data-stu-id="c16cf-169">General description of the React app</span></span>

<span data-ttu-id="c16cf-170">Obsługa uwierzytelniania i autoryzacji interfejsu API w szablonie reagowania znajduje się w katalogu *ClientApp\src\components\api-Authorization* .</span><span class="sxs-lookup"><span data-stu-id="c16cf-170">The support for authentication and API authorization in the React template resides in the *ClientApp\src\components\api-authorization* directory.</span></span> <span data-ttu-id="c16cf-171">Składa się z następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="c16cf-171">It's composed of the following elements:</span></span>

* <span data-ttu-id="c16cf-172">4 składniki:</span><span class="sxs-lookup"><span data-stu-id="c16cf-172">4 components:</span></span>
  * <span data-ttu-id="c16cf-173">*Login. js*: obsługuje przepływ logowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-173">*Login.js*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="c16cf-174">*Wyloguj. js*: obsługuje przepływ wylogowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-174">*Logout.js*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="c16cf-175">*LoginMenu. js*: element widget wyświetlający jeden z następujących zestawów linków:</span><span class="sxs-lookup"><span data-stu-id="c16cf-175">*LoginMenu.js*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="c16cf-176">Zarządzanie profilami użytkowników i wylogowywanie łączy podczas uwierzytelniania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c16cf-176">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="c16cf-177">Rejestrowanie i logowanie w przypadku braku uwierzytelnienia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c16cf-177">Registration and log in links when the user isn't authenticated.</span></span>
  * <span data-ttu-id="c16cf-178">*AuthorizeRoute. js*: składnik trasy, który wymaga uwierzytelnienia użytkownika przed renderowaniem składnika wskazanego w parametrze `Component`.</span><span class="sxs-lookup"><span data-stu-id="c16cf-178">*AuthorizeRoute.js*: A route component that requires a user to be authenticated before rendering the component indicated in the `Component` parameter.</span></span>
* <span data-ttu-id="c16cf-179">Wyeksportowane wystąpienie `authService` klasy `AuthorizeService`, które obsługuje szczegóły niższego poziomu procesu uwierzytelniania i ujawnia informacje o uwierzytelnionym użytkowniku w pozostałej części aplikacji do użycia.</span><span class="sxs-lookup"><span data-stu-id="c16cf-179">An exported `authService` instance of class `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>

<span data-ttu-id="c16cf-180">Teraz, gdy widzisz główne składniki rozwiązania, możesz zapoznać się ze szczegółowymi scenariuszami dotyczącymi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-180">Now that you've seen the main components of the solution, you can take a deeper look at individual scenarios for the app.</span></span>

## <a name="require-authorization-on-a-new-api"></a><span data-ttu-id="c16cf-181">Wymagaj autoryzacji na nowym interfejsie API</span><span class="sxs-lookup"><span data-stu-id="c16cf-181">Require authorization on a new API</span></span>

<span data-ttu-id="c16cf-182">Domyślnie system jest skonfigurowany tak, aby z łatwością wymagał autoryzacji dla nowych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="c16cf-182">By default, the system is configured to easily require authorization for new APIs.</span></span> <span data-ttu-id="c16cf-183">W tym celu Utwórz nowy kontroler i Dodaj atrybut `[Authorize]` do klasy Controller lub do dowolnej akcji w ramach kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c16cf-183">To do so, create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="customize-the-api-authentication-handler"></a><span data-ttu-id="c16cf-184">Dostosowywanie procedury obsługi uwierzytelniania interfejsu API</span><span class="sxs-lookup"><span data-stu-id="c16cf-184">Customize the API authentication handler</span></span>

<span data-ttu-id="c16cf-185">Aby dostosować konfigurację procedury obsługi JWT interfejsu API, skonfiguruj jej <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions> wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="c16cf-185">To customize the configuration of the API's JWT handler, configure its <xref:Microsoft.AspNetCore.Builder.JwtBearerOptions> instance:</span></span>

```csharp
services.AddAuthentication()
    .AddIdentityServerJwt();

services.Configure<JwtBearerOptions>(
    IdentityServerJwtConstants.IdentityServerJwtBearerScheme,
    options =>
    {
        ...
    });
```

<span data-ttu-id="c16cf-186">Procedura obsługi JWT interfejsu API wywołuje zdarzenia, które umożliwiają kontrolę nad procesem uwierzytelniania przy użyciu `JwtBearerEvents`.</span><span class="sxs-lookup"><span data-stu-id="c16cf-186">The API's JWT handler raises events that enable control over the authentication process using `JwtBearerEvents`.</span></span> <span data-ttu-id="c16cf-187">Aby zapewnić pomoc techniczną dla autoryzacji interfejsu API, `AddIdentityServerJwt` rejestruje własne procedury obsługi zdarzeń.</span><span class="sxs-lookup"><span data-stu-id="c16cf-187">To provide support for API authorization, `AddIdentityServerJwt` registers its own event handlers.</span></span>

<span data-ttu-id="c16cf-188">Aby dostosować obsługę zdarzenia, zawiń istniejący program obsługi zdarzeń z dodatkową logiką zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="c16cf-188">To customize the handling of an event, wrap the existing event handler with additional logic as required.</span></span> <span data-ttu-id="c16cf-189">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c16cf-189">For example:</span></span>

```csharp
services.Configure<JwtBearerOptions>(
    IdentityServerJwtConstants.IdentityServerJwtBearerScheme,
    options =>
    {
        var onTokenValidated = options.Events.OnTokenValidated;       
        
        options.Events.OnTokenValidated = async context =>
        {
            await onTokenValidated(context);
            ...
        }
    });
```

<span data-ttu-id="c16cf-190">W poprzednim kodzie program obsługi zdarzeń `OnTokenValidated` został zastąpiony implementacją niestandardową.</span><span class="sxs-lookup"><span data-stu-id="c16cf-190">In the preceding code, the `OnTokenValidated` event handler is replaced with a custom implementation.</span></span> <span data-ttu-id="c16cf-191">Ta implementacja:</span><span class="sxs-lookup"><span data-stu-id="c16cf-191">This implementation:</span></span>

1. <span data-ttu-id="c16cf-192">Wywołuje oryginalną implementację dostarczoną przez obsługę autoryzacji interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="c16cf-192">Calls the original implementation provided by the API authorization support.</span></span>
1. <span data-ttu-id="c16cf-193">Uruchom własną logikę niestandardową.</span><span class="sxs-lookup"><span data-stu-id="c16cf-193">Run its own custom logic.</span></span>

## <a name="protect-a-client-side-route-angular"></a><span data-ttu-id="c16cf-194">Ochrona trasy po stronie klienta (wartość kątowa)</span><span class="sxs-lookup"><span data-stu-id="c16cf-194">Protect a client-side route (Angular)</span></span>

<span data-ttu-id="c16cf-195">Ochrona trasy po stronie klienta odbywa się przez dodanie funkcji Autoryzuj ochronę do listy osłon do uruchomienia podczas konfigurowania trasy.</span><span class="sxs-lookup"><span data-stu-id="c16cf-195">Protecting a client-side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="c16cf-196">Przykładowo można zobaczyć, jak trasa `fetch-data` jest konfigurowana w module kątowym aplikacji głównej:</span><span class="sxs-lookup"><span data-stu-id="c16cf-196">As an example, you can see how the `fetch-data` route is configured within the main app Angular module:</span></span>

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="c16cf-197">Należy pamiętać, że ochrona trasy nie chroni rzeczywistego punktu końcowego (co nadal wymaga zastosowania atrybutu `[Authorize]`), ale uniemożliwia użytkownikowi przechodzenie do podanej trasy po stronie klienta, gdy nie jest ona uwierzytelniana.</span><span class="sxs-lookup"><span data-stu-id="c16cf-197">It's important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="c16cf-198">Uwierzytelnianie żądań interfejsu API (kątowy)</span><span class="sxs-lookup"><span data-stu-id="c16cf-198">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="c16cf-199">Żądania uwierzytelniające do interfejsów API hostowanych razem z aplikacją są wykonywane automatycznie przy użyciu interceptora klienta HTTP zdefiniowanego przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="c16cf-199">Authenticating requests to APIs hosted alongside the app is done automatically through the use of the HTTP client interceptor defined by the app.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="c16cf-200">Ochrona trasy po stronie klienta (reagowanie)</span><span class="sxs-lookup"><span data-stu-id="c16cf-200">Protect a client-side route (React)</span></span>

<span data-ttu-id="c16cf-201">Ochrona trasy po stronie klienta przy użyciu składnika `AuthorizeRoute` zamiast składnika zwykłego `Route`.</span><span class="sxs-lookup"><span data-stu-id="c16cf-201">Protect a client-side route by using the `AuthorizeRoute` component instead of the plain `Route` component.</span></span> <span data-ttu-id="c16cf-202">Na przykład Zwróć uwagę na sposób skonfigurowania trasy `fetch-data` w składniku `App`:</span><span class="sxs-lookup"><span data-stu-id="c16cf-202">For example, notice how the `fetch-data` route is configured within the `App` component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="c16cf-203">Ochrona trasy:</span><span class="sxs-lookup"><span data-stu-id="c16cf-203">Protecting a route:</span></span>

* <span data-ttu-id="c16cf-204">Nie chroni rzeczywistego punktu końcowego (co nadal wymaga zastosowania atrybutu `[Authorize]`).</span><span class="sxs-lookup"><span data-stu-id="c16cf-204">Doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it).</span></span>
* <span data-ttu-id="c16cf-205">Uniemożliwia użytkownikowi przechodzenie do danej trasy po stronie klienta, gdy nie jest ona uwierzytelniana.</span><span class="sxs-lookup"><span data-stu-id="c16cf-205">Only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="c16cf-206">Uwierzytelnianie żądań interfejsu API (reagowanie)</span><span class="sxs-lookup"><span data-stu-id="c16cf-206">Authenticate API requests (React)</span></span>

<span data-ttu-id="c16cf-207">Żądania uwierzytelniania przy użyciu reakcji są wykonywane najpierw przez zaimportowanie wystąpienia `authService` z `AuthorizeService`.</span><span class="sxs-lookup"><span data-stu-id="c16cf-207">Authenticating requests with React is done by first importing the `authService` instance from the `AuthorizeService`.</span></span> <span data-ttu-id="c16cf-208">Token dostępu jest pobierany z `authService` i jest dołączany do żądania, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="c16cf-208">The access token is retrieved from the `authService` and is attached to the request as shown below.</span></span> <span data-ttu-id="c16cf-209">W przypadku składników reagujących ta czynność jest wykonywana zwykle w metodzie `componentDidMount` cyklu życia lub w wyniku działania niektórych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="c16cf-209">In React components, this work is typically done in the `componentDidMount` lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="c16cf-210">Zaimportuj authService do składnika</span><span class="sxs-lookup"><span data-stu-id="c16cf-210">Import the authService into your component</span></span>

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="c16cf-211">Pobieranie i dołączanie tokenu dostępu do odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="c16cf-211">Retrieve and attach the access token to the response</span></span>

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

## <a name="deploy-to-production"></a><span data-ttu-id="c16cf-212">Wdrażanie w środowisku produkcyjnym</span><span class="sxs-lookup"><span data-stu-id="c16cf-212">Deploy to production</span></span>

<span data-ttu-id="c16cf-213">Aby wdrożyć aplikację w środowisku produkcyjnym, należy zainicjować następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="c16cf-213">To deploy the app to production, the following resources need to be provisioned:</span></span>

* <span data-ttu-id="c16cf-214">Baza danych do przechowywania kont użytkowników tożsamości i dotacji IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="c16cf-214">A database to store the Identity user accounts and the IdentityServer grants.</span></span>
* <span data-ttu-id="c16cf-215">Certyfikat produkcyjny do użycia na potrzeby podpisywania tokenów.</span><span class="sxs-lookup"><span data-stu-id="c16cf-215">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="c16cf-216">Nie ma określonych wymagań dotyczących tego certyfikatu; może to być certyfikat z podpisem własnym lub certyfikat obsługiwany przez urząd certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-216">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="c16cf-217">Można go wygenerować za poorednictwem standardowych narzędzi, takich jak PowerShell lub OpenSSL.</span><span class="sxs-lookup"><span data-stu-id="c16cf-217">It can be generated through standard tools like PowerShell or OpenSSL.</span></span>
  * <span data-ttu-id="c16cf-218">Można go zainstalować w magazynie certyfikatów na maszynach docelowych lub wdrożyć jako plik *PFX* przy użyciu silnego hasła.</span><span class="sxs-lookup"><span data-stu-id="c16cf-218">It can be installed into the certificate store on the target machines or deployed as a *.pfx* file with a strong password.</span></span>

### <a name="example-deploy-to-azure-websites"></a><span data-ttu-id="c16cf-219">Przykład: wdrażanie w usłudze Azure Websites</span><span class="sxs-lookup"><span data-stu-id="c16cf-219">Example: Deploy to Azure Websites</span></span>

<span data-ttu-id="c16cf-220">W tej sekcji opisano wdrażanie aplikacji w usłudze Azure Websites przy użyciu certyfikatu przechowywanego w magazynie certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="c16cf-220">This section describes deploying the app to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="c16cf-221">Aby zmodyfikować aplikację w celu załadowania certyfikatu z magazynu certyfikatów, plan App Service musi znajdować się co najmniej w warstwie Standardowa podczas konfigurowania w późniejszym kroku.</span><span class="sxs-lookup"><span data-stu-id="c16cf-221">To modify the app to load a certificate from the certificate store, the App Service plan needs to be on at least the Standard tier when you configure in a later step.</span></span> <span data-ttu-id="c16cf-222">W pliku *appSettings. JSON* aplikacji zmodyfikuj sekcję `IdentityServer`, aby uwzględnić szczegóły klucza:</span><span class="sxs-lookup"><span data-stu-id="c16cf-222">In the app's *appsettings.json* file, modify the `IdentityServer` section to include the key details:</span></span>

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

* <span data-ttu-id="c16cf-223">Nazwa magazynu reprezentuje nazwę magazynu certyfikatów, w którym przechowywany jest certyfikat.</span><span class="sxs-lookup"><span data-stu-id="c16cf-223">The store name represents the name of the certificate store where the certificate is stored.</span></span> <span data-ttu-id="c16cf-224">W tym przypadku wskazuje osobisty magazyn użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c16cf-224">In this case, it points to the personal user store.</span></span>
* <span data-ttu-id="c16cf-225">Lokalizacja magazynu reprezentuje miejsce, z którego ma zostać załadowany certyfikat (`CurrentUser` lub `LocalMachine`).</span><span class="sxs-lookup"><span data-stu-id="c16cf-225">The store location represents where to load the certificate from (`CurrentUser` or `LocalMachine`).</span></span>
* <span data-ttu-id="c16cf-226">Właściwość Name certyfikatu odpowiada podmiotowi wyróżnionemu dla certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="c16cf-226">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>

<span data-ttu-id="c16cf-227">Aby wdrożyć usługę Azure Websites, wdróż aplikację zgodnie z instrukcjami w temacie [wdrażanie aplikacji na platformie Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) w celu utworzenia niezbędnych zasobów platformy Azure i wdrożenia aplikacji w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="c16cf-227">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="c16cf-228">Po wykonaniu powyższych instrukcji aplikacja zostanie wdrożona na platformie Azure, ale nie jest jeszcze funkcjonalna.</span><span class="sxs-lookup"><span data-stu-id="c16cf-228">After following the preceding instructions, the app is deployed to Azure but isn't yet functional.</span></span> <span data-ttu-id="c16cf-229">Nadal trzeba skonfigurować certyfikat używany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="c16cf-229">The certificate used by the app still needs to be set up.</span></span> <span data-ttu-id="c16cf-230">Znajdź odcisk palca certyfikatu, który ma być używany, i postępuj zgodnie z instrukcjami opisanymi w artykule [ładowanie certyfikatów](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code).</span><span class="sxs-lookup"><span data-stu-id="c16cf-230">Locate the thumbprint for the certificate to be used, and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-the-certificate-in-code).</span></span>

<span data-ttu-id="c16cf-231">Chociaż te kroki zawierają informacje o protokole SSL, w portalu znajduje się sekcja **Certyfikaty prywatne** , w której można przekazać certyfikat z obsługą administracyjną do użycia z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="c16cf-231">While these steps mention SSL, there's a **Private certificates** section on the portal where you can upload the provisioned certificate to use with the app.</span></span>

<span data-ttu-id="c16cf-232">Po wykonaniu tego kroku należy ponownie uruchomić aplikację, która powinna działać.</span><span class="sxs-lookup"><span data-stu-id="c16cf-232">After this step, restart the app and it should be functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="c16cf-233">Inne opcje konfiguracji</span><span class="sxs-lookup"><span data-stu-id="c16cf-233">Other configuration options</span></span>

<span data-ttu-id="c16cf-234">Obsługa usługi API Authorization kompiluje się w oparciu o IdentityServer z zestawem Konwencji, wartościami domyślnymi i ulepszeniami, aby uprościć środowisko aplikacji jednostronicowych.</span><span class="sxs-lookup"><span data-stu-id="c16cf-234">The support for API authorization builds on top of IdentityServer with a set of conventions, default values, and enhancements to simplify the experience for SPAs.</span></span> <span data-ttu-id="c16cf-235">Niezbędna jest pełna moc IdentityServer w tle, jeśli integracja ASP.NET Core nie obejmuje Twojego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="c16cf-235">Needless to say, the full power of IdentityServer is available behind the scenes if the ASP.NET Core integrations don't cover your scenario.</span></span> <span data-ttu-id="c16cf-236">Pomoc techniczna ASP.NET Core jest skoncentrowana na aplikacjach "pierwszej strony", w których wszystkie aplikacje są tworzone i wdrażane przez naszą organizację.</span><span class="sxs-lookup"><span data-stu-id="c16cf-236">The ASP.NET Core support is focused on "first-party" apps, where all the apps are created and deployed by our organization.</span></span> <span data-ttu-id="c16cf-237">W związku z tym pomoc techniczna nie jest oferowana dla elementów, takich jak zgody lub Federacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-237">As such, support isn't offered for things like consent or federation.</span></span> <span data-ttu-id="c16cf-238">W tych scenariuszach Użyj IdentityServer i postępuj zgodnie z dokumentacją.</span><span class="sxs-lookup"><span data-stu-id="c16cf-238">For those scenarios, use IdentityServer and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="c16cf-239">Profile aplikacji</span><span class="sxs-lookup"><span data-stu-id="c16cf-239">Application profiles</span></span>

<span data-ttu-id="c16cf-240">Profile aplikacji są wstępnie zdefiniowanymi konfiguracjami dla aplikacji, które definiują ich parametry.</span><span class="sxs-lookup"><span data-stu-id="c16cf-240">Application profiles are predefined configurations for apps that further define their parameters.</span></span> <span data-ttu-id="c16cf-241">W tej chwili obsługiwane są następujące profile:</span><span class="sxs-lookup"><span data-stu-id="c16cf-241">At this time, the following profiles are supported:</span></span>

* <span data-ttu-id="c16cf-242">`IdentityServerSPA`: reprezentuje protokół SPA hostowany obok IdentityServer jako pojedynczą jednostkę.</span><span class="sxs-lookup"><span data-stu-id="c16cf-242">`IdentityServerSPA`: Represents a SPA hosted alongside IdentityServer as a single unit.</span></span>
  * <span data-ttu-id="c16cf-243">`redirect_uri` domyślnie `/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="c16cf-243">The `redirect_uri` defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="c16cf-244">`post_logout_redirect_uri` domyślnie `/authentication/logout-callback`.</span><span class="sxs-lookup"><span data-stu-id="c16cf-244">The `post_logout_redirect_uri` defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="c16cf-245">Zestaw zakresów zawiera `openid`, `profile`i każdy zakres zdefiniowany dla interfejsów API w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-245">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="c16cf-246">Zestaw dozwolonych typów odpowiedzi OIDC to `id_token token` lub każda z nich pojedynczo (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="c16cf-246">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="c16cf-247">Dozwolony tryb odpowiedzi to `fragment`.</span><span class="sxs-lookup"><span data-stu-id="c16cf-247">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="c16cf-248">`SPA`: reprezentuje SPA, który nie jest hostowany z IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="c16cf-248">`SPA`: Represents a SPA that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="c16cf-249">Zestaw zakresów zawiera `openid`, `profile`i każdy zakres zdefiniowany dla interfejsów API w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-249">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="c16cf-250">Zestaw dozwolonych typów odpowiedzi OIDC to `id_token token` lub każda z nich pojedynczo (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="c16cf-250">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="c16cf-251">Dozwolony tryb odpowiedzi to `fragment`.</span><span class="sxs-lookup"><span data-stu-id="c16cf-251">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="c16cf-252">`IdentityServerJwt`: reprezentuje interfejs API, który jest hostowany wraz z IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="c16cf-252">`IdentityServerJwt`: Represents an API that is hosted alongside with IdentityServer.</span></span>
  * <span data-ttu-id="c16cf-253">Aplikacja jest skonfigurowana tak, aby zawierała pojedynczy zakres, który jest wartością domyślną dla nazwy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-253">The app is configured to have a single scope that defaults to the app name.</span></span>
* <span data-ttu-id="c16cf-254">`API`: reprezentuje interfejs API, który nie jest obsługiwany przez IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="c16cf-254">`API`: Represents an API that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="c16cf-255">Aplikacja jest skonfigurowana tak, aby zawierała pojedynczy zakres, który jest wartością domyślną dla nazwy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-255">The app is configured to have a single scope that defaults to the app name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="c16cf-256">Konfiguracja za poorednictwem AppSettings</span><span class="sxs-lookup"><span data-stu-id="c16cf-256">Configuration through AppSettings</span></span>

<span data-ttu-id="c16cf-257">Skonfiguruj aplikacje za pomocą systemu konfiguracji, dodając je do listy `Clients` lub `Resources`.</span><span class="sxs-lookup"><span data-stu-id="c16cf-257">Configure the apps through the configuration system by adding them to the list of `Clients` or `Resources`.</span></span>

<span data-ttu-id="c16cf-258">Skonfiguruj `redirect_uri` i Właściwość `post_logout_redirect_uri` każdego klienta, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="c16cf-258">Configure each client's `redirect_uri` and `post_logout_redirect_uri` property, as shown in the following example:</span></span>

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

<span data-ttu-id="c16cf-259">Podczas konfigurowania zasobów można skonfigurować zakresy dla zasobu, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="c16cf-259">When configuring resources, you can configure the scopes for the resource as shown below:</span></span>

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

### <a name="configuration-through-code"></a><span data-ttu-id="c16cf-260">Konfiguracja przy użyciu kodu</span><span class="sxs-lookup"><span data-stu-id="c16cf-260">Configuration through code</span></span>

<span data-ttu-id="c16cf-261">Klientów i zasoby można również skonfigurować za pomocą kodu przy użyciu przeciążenia `AddApiAuthorization`, które wykonuje akcję konfigurowania opcji.</span><span class="sxs-lookup"><span data-stu-id="c16cf-261">You can also configure the clients and resources through code using an overload of `AddApiAuthorization` that takes an action to configure options.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="c16cf-262">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c16cf-262">Additional resources</span></span>

* <xref:spa/angular>
* <xref:spa/react>
* <xref:security/authentication/scaffold-identity>
