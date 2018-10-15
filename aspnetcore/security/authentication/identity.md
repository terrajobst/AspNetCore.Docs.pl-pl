---
title: Wprowadzenie do tożsamości programu ASP.NET Core
author: rick-anderson
description: Tożsamość za pomocą aplikacji ASP.NET Core. Dowiedz się, jak ustawić wymagania dotyczące hasła (RequireDigit, RequiredLength, RequiredUniqueChars i inne).
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: e0494afd807080f4cd71127abce7bdb93f952dca
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325513"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="bcc59-104">Wprowadzenie do tożsamości programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bcc59-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="bcc59-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bcc59-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bcc59-106">Tożsamość platformy ASP.NET Core jest systemu członkostwa, który dodaje funkcje logowania do aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bcc59-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="bcc59-107">Użytkownicy mogą tworzyć konta informacje logowania, przechowywane w tożsamość lub używają dostawcy logowania zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="bcc59-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="bcc59-108">Dostawcy logowania zewnętrznego obsługiwanych obejmują [Facebook, Google, Account Microsoft i Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="bcc59-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="bcc59-109">Tożsamości można skonfigurować przy użyciu bazy danych programu SQL Server do przechowywania nazwy użytkownika, hasła i dane profilu.</span><span class="sxs-lookup"><span data-stu-id="bcc59-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="bcc59-110">Alternatywnie innego magazynu trwałego można, na przykład usługa Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="bcc59-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

[<span data-ttu-id="bcc59-111">Wyświetlanie lub pobieranie przykładowego kodu.</span><span class="sxs-lookup"><span data-stu-id="bcc59-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="bcc59-112">(Jak pobrać)</span><span class="sxs-lookup"><span data-stu-id="bcc59-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

<span data-ttu-id="bcc59-113">W tym temacie możesz dowiedzieć się, jak zarejestrować, zaloguj się za pomocą tożsamości i wylogowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="bcc59-113">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="bcc59-114">Aby uzyskać bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji korzystających z tożsamości zobacz sekcję następne kroki na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="bcc59-114">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>
## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="bcc59-115">AddDefaultIdentity i AddIdentity</span><span class="sxs-lookup"><span data-stu-id="bcc59-115">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="bcc59-116">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) została wprowadzona w ASP. Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="bcc59-116">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.Core 2.1.</span></span> <span data-ttu-id="bcc59-117">Wywoływanie `AddDefaultIdentity` jest podobny do wywoływania następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="bcc59-117">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="bcc59-118">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="bcc59-118">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="bcc59-119">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="bcc59-119">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="bcc59-120">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="bcc59-120">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="bcc59-121">Zobacz [źródła AddDefaultIdentity](https://github.com/aspnet/Identity/blob/2634637fd535b229762b5e4a49cdd128f4d8f12e/src/UI/IdentityServiceCollectionUIExtensions.cs#L47-L64) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="bcc59-121">See [AddDefaultIdentity source](https://github.com/aspnet/Identity/blob/2634637fd535b229762b5e4a49cdd128f4d8f12e/src/UI/IdentityServiceCollectionUIExtensions.cs#L47-L64) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="bcc59-122">Tworzenie aplikacji sieci Web z uwierzytelnianiem</span><span class="sxs-lookup"><span data-stu-id="bcc59-122">Create a Web app with authentication</span></span>

<span data-ttu-id="bcc59-123">Tworzenie projektu aplikacji sieci Web programu ASP.NET Core z indywidualnymi kontami użytkowników.</span><span class="sxs-lookup"><span data-stu-id="bcc59-123">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bcc59-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bcc59-124">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bcc59-125">Wybierz **pliku** > **nowe** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="bcc59-125">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="bcc59-126">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="bcc59-126">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="bcc59-127">Nadaj projektowi nazwę **WebApp1** mieć tej samej przestrzeni nazw jako do pobrania projektu.</span><span class="sxs-lookup"><span data-stu-id="bcc59-127">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="bcc59-128">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="bcc59-128">Click **OK**.</span></span>
* <span data-ttu-id="bcc59-129">Wybierz platformy ASP.NET Core **aplikacji sieci Web** platformy ASP.NET Core 2.1, następnie wybierz **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="bcc59-129">Select an ASP.NET Core **Web Application** for ASP.NET Core 2.1, then select **Change Authentication**.</span></span>
* <span data-ttu-id="bcc59-130">Wybierz **indywidualne konta użytkowników** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="bcc59-130">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bcc59-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bcc59-131">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="bcc59-132">Wygenerowany projekt zawiera [tożsamości platformy ASP.NET Core](xref:security/authentication/identity) jako [biblioteki klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="bcc59-132">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="bcc59-133">Rejestrowanie testu i zaloguj się</span><span class="sxs-lookup"><span data-stu-id="bcc59-133">Test Register and Login</span></span>

<span data-ttu-id="bcc59-134">Uruchom aplikację i zarejestrować użytkownik.</span><span class="sxs-lookup"><span data-stu-id="bcc59-134">Run the app and register a user.</span></span> <span data-ttu-id="bcc59-135">Zależności od wielkości ekranu, może być konieczne wybrać przycisk przełączania nawigacji, aby wyświetlić **zarejestrować** i **logowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="bcc59-135">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

![Przełącz pasek nawigacyjny przycisku](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="bcc59-137">Konfigurowanie usługi zarządzania tożsamościami</span><span class="sxs-lookup"><span data-stu-id="bcc59-137">Configure Identity services</span></span>

<span data-ttu-id="bcc59-138">Usługi są dodawane w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bcc59-138">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="bcc59-139">Typowy wzorzec polega na wywołaniu wszystkich `Add{Service}` metody, a następnie wywołania wszystkich `services.Configure{Service}` metody.</span><span class="sxs-lookup"><span data-stu-id="bcc59-139">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span> <span data-ttu-id="bcc59-140">Poniższy kod nie zawiera wygenerowanego szablonu `CookiePolicyOptions`:</span><span class="sxs-lookup"><span data-stu-id="bcc59-140">The following code doesn't include the template generated `CookiePolicyOptions`:</span></span>

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="bcc59-141">Powyższy kod konfiguruje tożsamości przy użyciu wartości opcji domyślnych.</span><span class="sxs-lookup"><span data-stu-id="bcc59-141">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="bcc59-142">Usługi są dostępne dla aplikacji za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="bcc59-142">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="bcc59-143">Tożsamość jest włączona, wywołując [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="bcc59-143">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="bcc59-144">`UseAuthentication` dodaje uwierzytelniania [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="bcc59-144">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="bcc59-145">Usługi są dostępne dla aplikacji za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="bcc59-145">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="bcc59-146">Tożsamość jest włączone dla aplikacji, wywołując `UseAuthentication` w `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="bcc59-146">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="bcc59-147">`UseAuthentication` dodaje uwierzytelniania [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="bcc59-147">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="bcc59-148">Te usługi są dostępne dla aplikacji za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="bcc59-148">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="bcc59-149">Tożsamość jest włączone dla aplikacji, wywołując `UseIdentity` w `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="bcc59-149">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="bcc59-150">`UseIdentity` dodaje na podstawie plików cookie uwierzytelniania [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="bcc59-150">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="bcc59-151">Aby uzyskać więcej informacji, zobacz [klasy IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) i [uruchamiania aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="bcc59-151">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="bcc59-152">Zarejestruj szkieletu, logowania i wylogowania</span><span class="sxs-lookup"><span data-stu-id="bcc59-152">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="bcc59-153">Postępuj zgodnie z [tworzenia szkieletu tożsamości do projektu Razor z autoryzacją](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instrukcje.</span><span class="sxs-lookup"><span data-stu-id="bcc59-153">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bcc59-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bcc59-154">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bcc59-155">Dodaj pliki rejestru, logowania i wylogowania.</span><span class="sxs-lookup"><span data-stu-id="bcc59-155">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="bcc59-156">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="bcc59-156">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="bcc59-157">Jeśli utworzono projekt o nazwie **WebApp1**, uruchom następujące polecenia.</span><span class="sxs-lookup"><span data-stu-id="bcc59-157">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="bcc59-158">W przeciwnym razie użyj poprawną przestrzeń nazw dla `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="bcc59-158">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="bcc59-159">PowerShell używa średnika jako separatora polecenia.</span><span class="sxs-lookup"><span data-stu-id="bcc59-159">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="bcc59-160">Przy użyciu programu PowerShell, ucieczki średnikami, na liście plików lub umieścić na liście plików w podwójnym cudzysłowie, jak w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="bcc59-160">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="bcc59-161">Sprawdź Rejestr</span><span class="sxs-lookup"><span data-stu-id="bcc59-161">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="bcc59-162">Kiedy użytkownik kliknie **zarejestrować** linku `RegisterModel.OnPostAsync` akcja jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="bcc59-162">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="bcc59-163">Użytkownik jest tworzony przez [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) na `_userManager` obiektu.</span><span class="sxs-lookup"><span data-stu-id="bcc59-163">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="bcc59-164">`_userManager` znajduje się przez wstrzykiwanie zależności):</span><span class="sxs-lookup"><span data-stu-id="bcc59-164">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="bcc59-165">Kiedy użytkownik kliknie **zarejestrować** linku `Register` jest wywoływana Akcja `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="bcc59-165">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="bcc59-166">`Register` Akcja powoduje utworzenie użytkownika, wywołując `CreateAsync` na `_userManager` obiektu (udostępniane `AccountController` przez wstrzykiwanie zależności):</span><span class="sxs-lookup"><span data-stu-id="bcc59-166">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="bcc59-167">Jeśli użytkownik został pomyślnie utworzony, użytkownik jest zalogowany przez wywołanie `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="bcc59-167">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="bcc59-168">**Uwaga:** zobacz [konta potwierdzenia](xref:security/authentication/accconfirm#prevent-login-at-registration) kroki uniknąć natychmiastowego logowania podczas rejestracji.</span><span class="sxs-lookup"><span data-stu-id="bcc59-168">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="bcc59-169">Zaloguj się</span><span class="sxs-lookup"><span data-stu-id="bcc59-169">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bcc59-170">Zostanie wyświetlony formularz logowania po:</span><span class="sxs-lookup"><span data-stu-id="bcc59-170">The Login form is displayed when:</span></span>

* <span data-ttu-id="bcc59-171">**Zaloguj** łącze jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="bcc59-171">The **Log in** link is selected.</span></span>
* <span data-ttu-id="bcc59-172">Użytkownik próbuje uzyskać dostęp ograniczony strona, która nie wolno im dostępu **lub** gdy jeszcze nie został uwierzytelniony przez system.</span><span class="sxs-lookup"><span data-stu-id="bcc59-172">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="bcc59-173">Po przesłaniu formularza na stronie logowania `OnPostAsync` nosi nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="bcc59-173">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="bcc59-174">`PasswordSignInAsync` jest wywoływana w `_signInManager` obiektu (udostępnione przez wstrzykiwanie zależności).</span><span class="sxs-lookup"><span data-stu-id="bcc59-174">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="bcc59-175">Podstawa `Controller` klasy ujawnia `User` właściwość, której będziesz mieć dostęp z metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="bcc59-175">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="bcc59-176">Na przykład, można wyliczyć `User.Claims` i podejmowania decyzji dotyczących autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="bcc59-176">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="bcc59-177">Aby uzyskać więcej informacji, zobacz [autoryzacji](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="bcc59-177">For more information, see [Authorization](xref:security/authorization/index).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bcc59-178">Formularz logowania jest wyświetlane, gdy użytkownicy wybiorą **Zaloguj** połączenia lub nastąpi przekierowanie podczas uzyskiwania dostępu do strony, która wymaga uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="bcc59-178">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="bcc59-179">Gdy użytkownik przesyła formularz na stronie logowania `AccountController` `Login` nosi nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="bcc59-179">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="bcc59-180">`Login` Wywołania akcji `PasswordSignInAsync` na `_signInManager` obiektu (udostępniane `AccountController` przez wstrzykiwanie zależności).</span><span class="sxs-lookup"><span data-stu-id="bcc59-180">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="bcc59-181">Podstawa (`Controller` lub `PageModel`) klasy ujawnia `User` właściwości.</span><span class="sxs-lookup"><span data-stu-id="bcc59-181">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="bcc59-182">Na przykład `User.Claims` mogą być wyliczane w celu podejmowania decyzji dotyczących autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="bcc59-182">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="bcc59-183">Wyloguj</span><span class="sxs-lookup"><span data-stu-id="bcc59-183">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bcc59-184">**Wyloguj** wywołana `LogoutModel.OnPost` akcji.</span><span class="sxs-lookup"><span data-stu-id="bcc59-184">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

<span data-ttu-id="bcc59-185">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) czyści oświadczenia użytkownika przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="bcc59-185">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="bcc59-186">Nie przekierowania po wywołaniu `SignOutAsync` lub użytkownik będzie **nie** wylogowanie.</span><span class="sxs-lookup"><span data-stu-id="bcc59-186">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="bcc59-187">Wpis jest określona w *Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="bcc59-187">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="bcc59-188">Klikając **Wyloguj** link wywołania `LogOut` akcji.</span><span class="sxs-lookup"><span data-stu-id="bcc59-188">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="bcc59-189">Poprzedni kod wywołuje `_signInManager.SignOutAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="bcc59-189">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="bcc59-190">`SignOutAsync` Metoda czyści oświadczenia użytkownika przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="bcc59-190">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="bcc59-191">Testowanie tożsamości</span><span class="sxs-lookup"><span data-stu-id="bcc59-191">Test Identity</span></span>

<span data-ttu-id="bcc59-192">Domyślne szablony projektów sieci web zezwolić na dostęp anonimowy do strony głównej.</span><span class="sxs-lookup"><span data-stu-id="bcc59-192">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="bcc59-193">Aby sprawdzić tożsamość, należy dodać [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) do strony informacje.</span><span class="sxs-lookup"><span data-stu-id="bcc59-193">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the About page.</span></span>

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

<span data-ttu-id="bcc59-194">Jeśli użytkownik jest zalogowany, wyloguj się. Uruchom aplikację i wybierz **o** łącza.</span><span class="sxs-lookup"><span data-stu-id="bcc59-194">If you are signed in, sign out. Run the app and select the **About** link.</span></span> <span data-ttu-id="bcc59-195">Nastąpi przekierowanie do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="bcc59-195">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="bcc59-196">Zapoznaj się z tożsamości</span><span class="sxs-lookup"><span data-stu-id="bcc59-196">Explore Identity</span></span>

<span data-ttu-id="bcc59-197">Aby bardziej szczegółowo zapoznać się z tożsamości:</span><span class="sxs-lookup"><span data-stu-id="bcc59-197">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="bcc59-198">Tworzenie pełnej tożsamości interfejsu użytkownika źródła</span><span class="sxs-lookup"><span data-stu-id="bcc59-198">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="bcc59-199">Sprawdź źródła poszczególnych stron i krok po kroku debugera.</span><span class="sxs-lookup"><span data-stu-id="bcc59-199">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="bcc59-200">Składniki tożsamości</span><span class="sxs-lookup"><span data-stu-id="bcc59-200">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bcc59-201">Wszystkie tożsamości zależne pakiety NuGet są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bcc59-201">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="bcc59-202">Pakiet podstawowego dla tożsamości jest [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="bcc59-202">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="bcc59-203">Ten pakiet zawiera podstawowy zestaw interfejsów dla produktu ASP.NET Core Identity i jest dołączony przez `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="bcc59-203">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="bcc59-204">Migrowanie tożsamości platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bcc59-204">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="bcc59-205">Aby uzyskać więcej informacji oraz wskazówki dotyczące migrowania istniejących sklepu tożsamości, zobacz [migracji uwierzytelnianie i tożsamość](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="bcc59-205">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="bcc59-206">Ustawianie siły hasła</span><span class="sxs-lookup"><span data-stu-id="bcc59-206">Setting password strength</span></span>

<span data-ttu-id="bcc59-207">Zobacz [konfiguracji](#pw) przykład określająca wymagania dotyczące minimalnych hasła.</span><span class="sxs-lookup"><span data-stu-id="bcc59-207">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bcc59-208">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="bcc59-208">Next Steps</span></span>

* [<span data-ttu-id="bcc59-209">Konfigurowanie tożsamości</span><span class="sxs-lookup"><span data-stu-id="bcc59-209">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
