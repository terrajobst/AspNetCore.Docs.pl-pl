---
title: Zabezpieczanie ASP.NET Core aplikacji hostowanej Blazor webassembly z serwerem tożsamości
author: guardrex
description: Aby utworzyć nową aplikację hostowaną Blazor z uwierzytelnianiem z poziomu programu Visual Studio, który korzysta z zaplecza usługi [IdentityServer](https://identityserver.io/)
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/webassembly/hosted-with-identity-server
ms.openlocfilehash: 6c7942a827d88a620e6f295af3f523c23f4b3890
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219054"
---
# <a name="secure-an-aspnet-core-opno-locblazor-webassembly-hosted-app-with-identity-server"></a><span data-ttu-id="43803-103">Zabezpieczanie ASP.NET Core aplikacji hostowanej Blazor webassembly z serwerem tożsamości</span><span class="sxs-lookup"><span data-stu-id="43803-103">Secure an ASP.NET Core Blazor WebAssembly hosted app with Identity Server</span></span>

<span data-ttu-id="43803-104">Autorzy [Javier Calvarro Nelson](https://github.com/javiercn) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="43803-104">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

<span data-ttu-id="43803-105">Aby utworzyć nową aplikację hostowaną Blazor w programie Visual Studio, która używa [IdentityServer](https://identityserver.io/) do uwierzytelniania użytkowników i wywołań interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="43803-105">To create a new Blazor hosted app in Visual Studio that uses [IdentityServer](https://identityserver.io/) to authenticate users and API calls:</span></span>

1. <span data-ttu-id="43803-106">Użyj programu Visual Studio, aby utworzyć nową aplikację **Blazor webassembly** .</span><span class="sxs-lookup"><span data-stu-id="43803-106">Use Visual Studio to create a new **Blazor WebAssembly** app.</span></span> <span data-ttu-id="43803-107">Aby uzyskać więcej informacji, zobacz <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="43803-107">For more information, see <xref:blazor/get-started>.</span></span>
1. <span data-ttu-id="43803-108">W oknie dialogowym **Tworzenie nowej aplikacji Blazor** wybierz pozycję **Zmień** w sekcji **uwierzytelnianie** .</span><span class="sxs-lookup"><span data-stu-id="43803-108">In the **Create a new Blazor app** dialog, select **Change** in the **Authentication** section.</span></span>
1. <span data-ttu-id="43803-109">Wybierz **pojedyncze konta użytkowników** , a następnie **przycisk OK**.</span><span class="sxs-lookup"><span data-stu-id="43803-109">Select **Individual User Accounts** followed by **OK**.</span></span>
1. <span data-ttu-id="43803-110">Zaznacz pole wyboru **hostowane ASP.NET Core** w sekcji **Zaawansowane** .</span><span class="sxs-lookup"><span data-stu-id="43803-110">Select the **ASP.NET Core hosted** checkbox in the **Advanced** section.</span></span>
1. <span data-ttu-id="43803-111">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="43803-111">Select the **Create** button.</span></span>

<span data-ttu-id="43803-112">Aby utworzyć aplikację w powłoce poleceń, wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="43803-112">To create the app in a command shell, execute the following command:</span></span>

```dotnetcli
dotnet new blazorwasm -au Individual -ho
```

<span data-ttu-id="43803-113">Aby określić lokalizację wyjściową, która tworzy folder projektu, jeśli nie istnieje, Uwzględnij opcję Output w poleceniu z ścieżką (na przykład `-o BlazorSample`).</span><span class="sxs-lookup"><span data-stu-id="43803-113">To specify the output location, which creates a project folder if it doesn't exist, include the output option in the command with a path (for example, `-o BlazorSample`).</span></span> <span data-ttu-id="43803-114">Nazwa folderu jest również częścią nazwy projektu.</span><span class="sxs-lookup"><span data-stu-id="43803-114">The folder name also becomes part of the project's name.</span></span>

## <a name="server-app-configuration"></a><span data-ttu-id="43803-115">Konfiguracja aplikacji serwera</span><span class="sxs-lookup"><span data-stu-id="43803-115">Server app configuration</span></span>

<span data-ttu-id="43803-116">W poniższych sekcjach opisano Dodatki do projektu w przypadku włączenia obsługi uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="43803-116">The following sections describe additions to the project when authentication support is included.</span></span>

### <a name="startup-class"></a><span data-ttu-id="43803-117">Klasa początkowa</span><span class="sxs-lookup"><span data-stu-id="43803-117">Startup class</span></span>

<span data-ttu-id="43803-118">Klasa `Startup` ma następujące dodatki:</span><span class="sxs-lookup"><span data-stu-id="43803-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="43803-119">W pliku `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="43803-119">In `Startup.ConfigureServices`:</span></span>

  * <span data-ttu-id="43803-120">Tożsamość z domyślnym interfejsem użytkownika:</span><span class="sxs-lookup"><span data-stu-id="43803-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="43803-121">IdentityServer z dodatkową metodą pomocniczą <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A>, która konfiguruje niektóre domyślne konwencje ASP.NET Core w oparciu o IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="43803-121">IdentityServer with an additional <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method that sets up some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="43803-122">Uwierzytelnianie za pomocą dodatkowej metody pomocnika <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A>, która konfiguruje aplikację do weryfikowania tokenów JWT utworzonych przez IdentityServer:</span><span class="sxs-lookup"><span data-stu-id="43803-122">Authentication with an additional <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="43803-123">W pliku `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="43803-123">In `Startup.Configure`:</span></span>

  * <span data-ttu-id="43803-124">Oprogramowanie pośredniczące uwierzytelniania odpowiedzialne za Weryfikowanie poświadczeń żądania i Ustawianie użytkownika w kontekście żądania:</span><span class="sxs-lookup"><span data-stu-id="43803-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="43803-125">Oprogramowanie pośredniczące IdentityServer, które uwidacznia punkty końcowe połączenia Open ID Connect (OIDC):</span><span class="sxs-lookup"><span data-stu-id="43803-125">The IdentityServer middleware that exposes the Open ID Connect (OIDC) endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="43803-126">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="43803-126">AddApiAuthorization</span></span>

<span data-ttu-id="43803-127">Metoda pomocnika <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> konfiguruje [IdentityServer](https://identityserver.io/) dla scenariuszy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="43803-127">The <xref:Microsoft.Extensions.DependencyInjection.IdentityServerBuilderConfigurationExtensions.AddApiAuthorization%2A> helper method configures [IdentityServer](https://identityserver.io/) for ASP.NET Core scenarios.</span></span> <span data-ttu-id="43803-128">IdentityServer to zaawansowane i rozszerzalne środowisko do obsługi zagadnień związanych z zabezpieczeniami aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43803-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="43803-129">IdentityServer ujawnia niepotrzebną złożoność w najbardziej typowych scenariuszach.</span><span class="sxs-lookup"><span data-stu-id="43803-129">IdentityServer exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="43803-130">W związku z tym zestaw Konwencji i opcji konfiguracji jest dostępny, ponieważ rozważamy dobry punkt wyjścia.</span><span class="sxs-lookup"><span data-stu-id="43803-130">Consequently, a set of conventions and configuration options is provided that we consider a good starting point.</span></span> <span data-ttu-id="43803-131">Gdy uwierzytelnianie będzie wymagało zmiany, pełne możliwości IdentityServer są nadal dostępne, aby dostosować uwierzytelnianie zgodnie z wymaganiami aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43803-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit an app's requirements.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="43803-132">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="43803-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="43803-133">Metoda pomocnika <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> konfiguruje schemat zasad dla aplikacji jako domyślną procedurę obsługi uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="43803-133">The <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="43803-134">Zasady są skonfigurowane tak, aby zezwalać na tożsamość do obsługi wszystkich żądań kierowanych do dowolnej ścieżki podrzędnej w obszarze adresu URL tożsamości `/Identity`.</span><span class="sxs-lookup"><span data-stu-id="43803-134">The policy is configured to allow Identity to handle all requests routed to any subpath in the Identity URL space `/Identity`.</span></span> <span data-ttu-id="43803-135"><xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> obsługuje wszystkie inne żądania.</span><span class="sxs-lookup"><span data-stu-id="43803-135">The <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> handles all other requests.</span></span> <span data-ttu-id="43803-136">Ponadto ta metoda:</span><span class="sxs-lookup"><span data-stu-id="43803-136">Additionally, this method:</span></span>

* <span data-ttu-id="43803-137">Rejestruje zasób interfejsu API `{APPLICATION NAME}API` z IdentityServer z domyślnym zakresem `{APPLICATION NAME}API`.</span><span class="sxs-lookup"><span data-stu-id="43803-137">Registers an `{APPLICATION NAME}API` API resource with IdentityServer with a default scope of `{APPLICATION NAME}API`.</span></span>
* <span data-ttu-id="43803-138">Konfiguruje oprogramowanie pośredniczące tokenu okaziciela JWT do weryfikowania tokenów wystawionych przez IdentityServer dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43803-138">Configures the JWT Bearer Token Middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="weatherforecastcontroller"></a><span data-ttu-id="43803-139">WeatherForecastController</span><span class="sxs-lookup"><span data-stu-id="43803-139">WeatherForecastController</span></span>

<span data-ttu-id="43803-140">W `WeatherForecastController` (*controllers/WeatherForecastController. cs*) atrybut [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) jest stosowany do klasy.</span><span class="sxs-lookup"><span data-stu-id="43803-140">In the `WeatherForecastController` (*Controllers/WeatherForecastController.cs*), the [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) attribute is applied to the class.</span></span> <span data-ttu-id="43803-141">Ten atrybut wskazuje, że użytkownik musi być autoryzowany na podstawie domyślnych zasad dostępu do zasobu.</span><span class="sxs-lookup"><span data-stu-id="43803-141">The attribute indicates that the user must be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="43803-142">Domyślne zasady autoryzacji są skonfigurowane tak, aby korzystały z domyślnego schematu uwierzytelniania, który jest konfigurowany przez <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> do schematu zasad, który został wymieniony wcześniej.</span><span class="sxs-lookup"><span data-stu-id="43803-142">The default authorization policy is configured to use the default authentication scheme, which is set up by <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilderExtensions.AddIdentityServerJwt%2A> to the policy scheme that was mentioned earlier.</span></span> <span data-ttu-id="43803-143">Metoda pomocnika służy do konfigurowania <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> jako domyślnej procedury obsługi żądań do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43803-143">The helper method configures <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerHandler> as the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="43803-144">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="43803-144">ApplicationDbContext</span></span>

<span data-ttu-id="43803-145">W `ApplicationDbContext` (*Data/ApplicationDbContext. cs*) ten sam <xref:Microsoft.EntityFrameworkCore.DbContext> jest używany w tożsamości z wyjątkiem, który rozszerza <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> w celu uwzględnienia schematu IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="43803-145">In the `ApplicationDbContext` (*Data/ApplicationDbContext.cs*), the same <xref:Microsoft.EntityFrameworkCore.DbContext> is used in Identity with the exception that it extends <xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> to include the schema for IdentityServer.</span></span> <span data-ttu-id="43803-146"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> pochodzi od <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.</span><span class="sxs-lookup"><span data-stu-id="43803-146"><xref:Microsoft.AspNetCore.ApiAuthorization.IdentityServer.ApiAuthorizationDbContext%601> is derived from <xref:Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext>.</span></span>

<span data-ttu-id="43803-147">Aby uzyskać pełną kontrolę nad schematem bazy danych, Dziedzicz z jednej z dostępnych <xref:Microsoft.EntityFrameworkCore.DbContext> klas i skonfiguruj kontekst, aby uwzględnić schemat tożsamości przez wywołanie `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` w metodzie `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="43803-147">To gain full control of the database schema, inherit from one of the available Identity <xref:Microsoft.EntityFrameworkCore.DbContext> classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` in the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="43803-148">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="43803-148">OidcConfigurationController</span></span>

<span data-ttu-id="43803-149">W `OidcConfigurationController` (*controllers/OidcConfigurationController. cs*) punkt końcowy klienta jest inicjowany do obsługi parametrów OIDC.</span><span class="sxs-lookup"><span data-stu-id="43803-149">In the `OidcConfigurationController` (*Controllers/OidcConfigurationController.cs*), the client endpoint is provisioned to serve OIDC parameters.</span></span>

### <a name="app-settings-files"></a><span data-ttu-id="43803-150">Pliki ustawień aplikacji</span><span class="sxs-lookup"><span data-stu-id="43803-150">App settings files</span></span>

<span data-ttu-id="43803-151">W sekcji `IdentityServer` pliku ustawień aplikacji (*appSettings. JSON*) w katalogu głównym projektu opisano listę skonfigurowanych klientów.</span><span class="sxs-lookup"><span data-stu-id="43803-151">In the app settings file (*appsettings.json*) at the project root, the `IdentityServer` section describes the list of configured clients.</span></span> <span data-ttu-id="43803-152">W poniższym przykładzie istnieje pojedynczy klient.</span><span class="sxs-lookup"><span data-stu-id="43803-152">In the following example, there's a single client.</span></span> <span data-ttu-id="43803-153">Nazwa klienta odpowiada nazwie aplikacji i jest zamapowana według Konwencji do parametru `ClientId` protokołu OAuth.</span><span class="sxs-lookup"><span data-stu-id="43803-153">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="43803-154">Profil wskazuje konfigurowany typ aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43803-154">The profile indicates the app type being configured.</span></span> <span data-ttu-id="43803-155">Profil jest używany wewnętrznie w celu napędu Konwencji upraszczających proces konfiguracji serwera.</span><span class="sxs-lookup"><span data-stu-id="43803-155">The profile is used internally to drive conventions that simplify the configuration process for the server.</span></span> <!-- There are several profiles available, as explained in the [Application profiles](#application-profiles) section. -->

```json
"IdentityServer": {
  "Clients": {
    "BlazorApplicationWithAuthentication.Client": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

<span data-ttu-id="43803-156">W pliku ustawień aplikacji środowiska programistycznego (*appSettings. Development. JSON*) w katalogu głównym projektu sekcja `IdentityServer` opisuje klucz używany do podpisywania tokenów.</span><span class="sxs-lookup"><span data-stu-id="43803-156">In the Development environment app settings file (*appsettings.Development.json*) at the project root, the `IdentityServer` section describes the key used to sign tokens.</span></span> <!-- When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section. -->

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="client-app-configuration"></a><span data-ttu-id="43803-157">Konfiguracja aplikacji klienta</span><span class="sxs-lookup"><span data-stu-id="43803-157">Client app configuration</span></span>

### <a name="authentication-package"></a><span data-ttu-id="43803-158">Pakiet uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="43803-158">Authentication package</span></span>

<span data-ttu-id="43803-159">Gdy aplikacja zostanie utworzona w celu używania poszczególnych kont użytkowników (`Individual`), aplikacja automatycznie otrzymuje odwołanie do pakietu dla `Microsoft.AspNetCore.Components.WebAssembly.Authentication` pakietu w pliku projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43803-159">When an app is created to use Individual User Accounts (`Individual`), the app automatically receives a package reference for the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package in the app's project file.</span></span> <span data-ttu-id="43803-160">Pakiet zawiera zestaw elementów podstawowych, które ułatwiają aplikacji uwierzytelnianie użytkowników i uzyskiwanie tokenów do wywoływania chronionych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="43803-160">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="43803-161">W przypadku dodawania uwierzytelniania do aplikacji ręcznie Dodaj pakiet do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="43803-161">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference 
    Include="Microsoft.AspNetCore.Components.WebAssembly.Authentication" 
    Version="{VERSION}" />
```

<span data-ttu-id="43803-162">Zastąp `{VERSION}` w powyższym odwołaniu do pakietu w wersji pakietu `Microsoft.AspNetCore.Blazor.Templates` pokazanej w <xref:blazor/get-started> artykule.</span><span class="sxs-lookup"><span data-stu-id="43803-162">Replace `{VERSION}` in the preceding package reference with the version of the `Microsoft.AspNetCore.Blazor.Templates` package shown in the <xref:blazor/get-started> article.</span></span>

### <a name="api-authorization-support"></a><span data-ttu-id="43803-163">Obsługa autoryzacji interfejsu API</span><span class="sxs-lookup"><span data-stu-id="43803-163">API authorization support</span></span>

<span data-ttu-id="43803-164">Obsługa uwierzytelniania użytkowników jest podłączona do kontenera usługi przez metodę rozszerzenia dostarczoną w pakiecie `Microsoft.AspNetCore.Components.WebAssembly.Authentication`.</span><span class="sxs-lookup"><span data-stu-id="43803-164">The support for authenticating users is plugged into the service container by the extension method provided inside the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span> <span data-ttu-id="43803-165">Ta metoda konfiguruje wszystkie usługi, które są konieczne, aby aplikacja mogła współdziałać z istniejącym systemem autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="43803-165">This method sets up all the services needed for the app to interact with the existing authorization system.</span></span>

```csharp
builder.Services.AddApiAuthorization();
```

<span data-ttu-id="43803-166">Domyślnie ładuje on konfigurację aplikacji według Konwencji z `_configuration/{client-id}`.</span><span class="sxs-lookup"><span data-stu-id="43803-166">By default, it loads the configuration for the app by convention from `_configuration/{client-id}`.</span></span> <span data-ttu-id="43803-167">Zgodnie z Konwencją identyfikator klienta jest ustawiany na nazwę zestawu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43803-167">By convention, the client ID is set to the app's assembly name.</span></span> <span data-ttu-id="43803-168">Ten adres URL można zmienić tak, aby wskazywał osobny punkt końcowy przez wywołanie przeciążenia z opcjami.</span><span class="sxs-lookup"><span data-stu-id="43803-168">This URL can be changed to point to a separate endpoint by calling the overload with options.</span></span>

### <a name="index-page"></a><span data-ttu-id="43803-169">Strona indeksu</span><span class="sxs-lookup"><span data-stu-id="43803-169">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-authentication.md)]

### <a name="app-component"></a><span data-ttu-id="43803-170">Składnik aplikacji</span><span class="sxs-lookup"><span data-stu-id="43803-170">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a><span data-ttu-id="43803-171">Składnik RedirectToLogin</span><span class="sxs-lookup"><span data-stu-id="43803-171">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a><span data-ttu-id="43803-172">Składnik LoginDisplay</span><span class="sxs-lookup"><span data-stu-id="43803-172">LoginDisplay component</span></span>

<span data-ttu-id="43803-173">Składnik `LoginDisplay` (*Shared/LoginDisplay. Razor*) jest renderowany w składniku `MainLayout` (*Shared/MainLayout. Razor*) i zarządza następującymi zachowaniami:</span><span class="sxs-lookup"><span data-stu-id="43803-173">The `LoginDisplay` component (*Shared/LoginDisplay.razor*) is rendered in the `MainLayout` component (*Shared/MainLayout.razor*) and manages the following behaviors:</span></span>

* <span data-ttu-id="43803-174">Dla uwierzytelnionych użytkowników:</span><span class="sxs-lookup"><span data-stu-id="43803-174">For authenticated users:</span></span>
  * <span data-ttu-id="43803-175">Wyświetla bieżącą nazwę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="43803-175">Displays the current user name.</span></span>
  * <span data-ttu-id="43803-176">Oferuje link do strony profilu użytkownika w ASP.NET Core tożsamość.</span><span class="sxs-lookup"><span data-stu-id="43803-176">Offers a link to the user profile page in ASP.NET Core Identity.</span></span>
  * <span data-ttu-id="43803-177">Oferuje przycisk umożliwiający wylogowanie się z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43803-177">Offers a button to log out of the app.</span></span>
* <span data-ttu-id="43803-178">Dla użytkowników anonimowych:</span><span class="sxs-lookup"><span data-stu-id="43803-178">For anonymous users:</span></span>
  * <span data-ttu-id="43803-179">Oferuje opcję rejestracji.</span><span class="sxs-lookup"><span data-stu-id="43803-179">Offers the option to register.</span></span>
  * <span data-ttu-id="43803-180">Oferuje opcję logowania.</span><span class="sxs-lookup"><span data-stu-id="43803-180">Offers the option to log in.</span></span>

```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager

<AuthorizeView>
    <Authorized>
        <a href="authentication/profile">Hello, @context.User.Identity.Name!</a>
        <button class="nav-link btn btn-link" @onclick="BeginSignOut">
            Log out
        </button>
    </Authorized>
    <NotAuthorized>
        <a href="authentication/register">Register</a>
        <a href="authentication/login">Log in</a>
    </NotAuthorized>
</AuthorizeView>

@code {
    private async Task BeginSignOut(MouseEventArgs args)
    {
        await SignOutManager.SetSignOutState();
        Navigation.NavigateTo("authentication/logout");
    }
}
```

### <a name="authentication-component"></a><span data-ttu-id="43803-181">Składnik uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="43803-181">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a><span data-ttu-id="43803-182">Składnik FetchData</span><span class="sxs-lookup"><span data-stu-id="43803-182">FetchData component</span></span>

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a><span data-ttu-id="43803-183">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="43803-183">Run the app</span></span>

<span data-ttu-id="43803-184">Uruchom aplikację z projektu serwera.</span><span class="sxs-lookup"><span data-stu-id="43803-184">Run the app from the Server project.</span></span> <span data-ttu-id="43803-185">W przypadku korzystania z programu Visual Studio wybierz projekt serwera w **Eksplorator rozwiązań** a następnie wybierz przycisk **Uruchom** na pasku narzędzi lub Uruchom aplikację z menu **Debuguj** .</span><span class="sxs-lookup"><span data-stu-id="43803-185">When using Visual Studio, select the Server project in **Solution Explorer** and select the **Run** button in the toolbar or start the app from the **Debug** menu.</span></span>

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]
