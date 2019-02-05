---
title: Wprowadzenie do tożsamości programu ASP.NET Core
author: rick-anderson
description: Tożsamość za pomocą aplikacji ASP.NET Core. Dowiedz się, jak ustawić wymagania dotyczące hasła (RequireDigit, RequiredLength, RequiredUniqueChars i inne).
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: 1a4e7fb3ac6a767ca17127dd58a9b9e65ed9a00b
ms.sourcegitcommit: e418cb9cddeb3de06fa0cb4fdb5529da03ff6d63
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55739686"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="1753b-104">Wprowadzenie do tożsamości programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1753b-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="1753b-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1753b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1753b-106">Tożsamość platformy ASP.NET Core jest systemu członkostwa, który dodaje funkcje logowania do aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1753b-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="1753b-107">Użytkownicy mogą tworzyć konta informacje logowania, przechowywane w tożsamość lub używają dostawcy logowania zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="1753b-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="1753b-108">Dostawcy logowania zewnętrznego obsługiwanych obejmują [Facebook, Google, Account Microsoft i Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="1753b-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="1753b-109">Tożsamości można skonfigurować przy użyciu bazy danych programu SQL Server do przechowywania nazwy użytkownika, hasła i dane profilu.</span><span class="sxs-lookup"><span data-stu-id="1753b-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="1753b-110">Alternatywnie innego magazynu trwałego można, na przykład usługa Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="1753b-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="1753b-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([sposobu pobierania)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="1753b-111">[View or download the sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="1753b-112">W tym temacie możesz dowiedzieć się, jak zarejestrować, zaloguj się za pomocą tożsamości i wylogowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1753b-112">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="1753b-113">Aby uzyskać bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji korzystających z tożsamości zobacz sekcję następne kroki na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="1753b-113">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>
## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="1753b-114">AddDefaultIdentity i AddIdentity</span><span class="sxs-lookup"><span data-stu-id="1753b-114">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="1753b-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) wprowadzono w programie ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="1753b-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="1753b-116">Wywoływanie `AddDefaultIdentity` jest podobny do wywoływania następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="1753b-116">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="1753b-117">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="1753b-117">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="1753b-118">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="1753b-118">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="1753b-119">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="1753b-119">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="1753b-120">Zobacz [źródła AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="1753b-120">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="1753b-121">Tworzenie aplikacji sieci Web z uwierzytelnianiem</span><span class="sxs-lookup"><span data-stu-id="1753b-121">Create a Web app with authentication</span></span>

<span data-ttu-id="1753b-122">Tworzenie projektu aplikacji sieci Web programu ASP.NET Core z indywidualnymi kontami użytkowników.</span><span class="sxs-lookup"><span data-stu-id="1753b-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1753b-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1753b-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1753b-124">Wybierz **pliku** > **nowe** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="1753b-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="1753b-125">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1753b-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="1753b-126">Nadaj projektowi nazwę **WebApp1** mieć tej samej przestrzeni nazw jako do pobrania projektu.</span><span class="sxs-lookup"><span data-stu-id="1753b-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="1753b-127">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="1753b-127">Click **OK**.</span></span>
* <span data-ttu-id="1753b-128">Wybierz platformy ASP.NET Core **aplikacji sieci Web**, a następnie wybierz **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="1753b-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="1753b-129">Wybierz **indywidualne konta użytkowników** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="1753b-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1753b-130">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1753b-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="1753b-131">Wygenerowany projekt zawiera [tożsamości platformy ASP.NET Core](xref:security/authentication/identity) jako [biblioteki klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="1753b-131">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="1753b-132">Biblioteki klas Razor tożsamości uwidacznia punkty końcowe z `Identity` obszaru.</span><span class="sxs-lookup"><span data-stu-id="1753b-132">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="1753b-133">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1753b-133">For example:</span></span>

* <span data-ttu-id="1753b-134">/ Tożsamość/konta/logowania</span><span class="sxs-lookup"><span data-stu-id="1753b-134">/Identity/Account/Login</span></span>
* <span data-ttu-id="1753b-135">/ Tożsamość/konto/wylogowania</span><span class="sxs-lookup"><span data-stu-id="1753b-135">/Identity/Account/Logout</span></span>
* <span data-ttu-id="1753b-136">/ Tożsamość/konto/Zarządzanie</span><span class="sxs-lookup"><span data-stu-id="1753b-136">/Identity/Account/Manage</span></span>

### <a name="test-register-and-login"></a><span data-ttu-id="1753b-137">Rejestrowanie testu i zaloguj się</span><span class="sxs-lookup"><span data-stu-id="1753b-137">Test Register and Login</span></span>

<span data-ttu-id="1753b-138">Uruchom aplikację i zarejestrować użytkownik.</span><span class="sxs-lookup"><span data-stu-id="1753b-138">Run the app and register a user.</span></span> <span data-ttu-id="1753b-139">Zależności od wielkości ekranu, może być konieczne wybrać przycisk przełączania nawigacji, aby wyświetlić **zarejestrować** i **logowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="1753b-139">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a><span data-ttu-id="1753b-140">Konfigurowanie usługi zarządzania tożsamościami</span><span class="sxs-lookup"><span data-stu-id="1753b-140">Configure Identity services</span></span>

<span data-ttu-id="1753b-141">Usługi są dodawane w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1753b-141">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="1753b-142">Typowy wzorzec polega na wywołaniu wszystkich `Add{Service}` metody, a następnie wywołania wszystkich `services.Configure{Service}` metody.</span><span class="sxs-lookup"><span data-stu-id="1753b-142">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="1753b-143">Powyższy kod konfiguruje tożsamości przy użyciu wartości opcji domyślnych.</span><span class="sxs-lookup"><span data-stu-id="1753b-143">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="1753b-144">Usługi są dostępne dla aplikacji za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1753b-144">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="1753b-145">Tożsamość jest włączona, wywołując [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="1753b-145">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="1753b-146">`UseAuthentication` dodaje uwierzytelniania [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="1753b-146">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="1753b-147">Usługi są dostępne dla aplikacji za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1753b-147">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="1753b-148">Tożsamość jest włączone dla aplikacji, wywołując `UseAuthentication` w `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="1753b-148">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="1753b-149">`UseAuthentication` dodaje uwierzytelniania [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="1753b-149">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="1753b-150">Te usługi są dostępne dla aplikacji za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1753b-150">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="1753b-151">Tożsamość jest włączone dla aplikacji, wywołując `UseIdentity` w `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="1753b-151">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="1753b-152">`UseIdentity` dodaje na podstawie plików cookie uwierzytelniania [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="1753b-152">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="1753b-153">Aby uzyskać więcej informacji, zobacz [klasy IdentityOptions](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) i [uruchamiania aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="1753b-153">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="1753b-154">Zarejestruj szkieletu, logowania i wylogowania</span><span class="sxs-lookup"><span data-stu-id="1753b-154">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="1753b-155">Postępuj zgodnie z [tworzenia szkieletu tożsamości do projektu Razor z autoryzacją](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instrukcjami, aby wygenerować kod przedstawiony w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="1753b-155">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1753b-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1753b-156">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1753b-157">Dodaj pliki rejestru, logowania i wylogowania.</span><span class="sxs-lookup"><span data-stu-id="1753b-157">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1753b-158">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="1753b-158">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1753b-159">Jeśli utworzono projekt o nazwie **WebApp1**, uruchom następujące polecenia.</span><span class="sxs-lookup"><span data-stu-id="1753b-159">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="1753b-160">W przeciwnym razie użyj poprawną przestrzeń nazw dla `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="1753b-160">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="1753b-161">PowerShell używa średnika jako separatora polecenia.</span><span class="sxs-lookup"><span data-stu-id="1753b-161">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="1753b-162">Przy użyciu programu PowerShell, ucieczki średnikami, na liście plików lub umieścić na liście plików w podwójnym cudzysłowie, jak w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="1753b-162">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="1753b-163">Sprawdź Rejestr</span><span class="sxs-lookup"><span data-stu-id="1753b-163">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="1753b-164">Kiedy użytkownik kliknie **zarejestrować** linku `RegisterModel.OnPostAsync` akcja jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="1753b-164">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="1753b-165">Użytkownik jest tworzony przez [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) na `_userManager` obiektu.</span><span class="sxs-lookup"><span data-stu-id="1753b-165">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="1753b-166">`_userManager` znajduje się przez wstrzykiwanie zależności):</span><span class="sxs-lookup"><span data-stu-id="1753b-166">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="1753b-167">Kiedy użytkownik kliknie **zarejestrować** linku `Register` jest wywoływana Akcja `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="1753b-167">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="1753b-168">`Register` Akcja powoduje utworzenie użytkownika, wywołując `CreateAsync` na `_userManager` obiektu (udostępniane `AccountController` przez wstrzykiwanie zależności):</span><span class="sxs-lookup"><span data-stu-id="1753b-168">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="1753b-169">Jeśli użytkownik został pomyślnie utworzony, użytkownik jest zalogowany przez wywołanie `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="1753b-169">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="1753b-170">**Uwaga:** Zobacz [konta potwierdzenia](xref:security/authentication/accconfirm#prevent-login-at-registration) kroki uniknąć natychmiastowego logowania podczas rejestracji.</span><span class="sxs-lookup"><span data-stu-id="1753b-170">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="1753b-171">Zaloguj się</span><span class="sxs-lookup"><span data-stu-id="1753b-171">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1753b-172">Zostanie wyświetlony formularz logowania po:</span><span class="sxs-lookup"><span data-stu-id="1753b-172">The Login form is displayed when:</span></span>

* <span data-ttu-id="1753b-173">**Zaloguj** łącze jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="1753b-173">The **Log in** link is selected.</span></span>
* <span data-ttu-id="1753b-174">Użytkownik próbuje uzyskać dostęp ograniczony strona, która nie wolno im dostępu **lub** gdy jeszcze nie został uwierzytelniony przez system.</span><span class="sxs-lookup"><span data-stu-id="1753b-174">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="1753b-175">Po przesłaniu formularza na stronie logowania `OnPostAsync` nosi nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="1753b-175">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="1753b-176">`PasswordSignInAsync` jest wywoływana w `_signInManager` obiektu (udostępnione przez wstrzykiwanie zależności).</span><span class="sxs-lookup"><span data-stu-id="1753b-176">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="1753b-177">Podstawa `Controller` klasy ujawnia `User` właściwość, której będziesz mieć dostęp z metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="1753b-177">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="1753b-178">Na przykład, można wyliczyć `User.Claims` i podejmowania decyzji dotyczących autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="1753b-178">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="1753b-179">Aby uzyskać więcej informacji, zobacz <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="1753b-179">For more information, see <xref:security/authorization/introduction>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1753b-180">Formularz logowania jest wyświetlane, gdy użytkownicy wybiorą **Zaloguj** połączenia lub nastąpi przekierowanie podczas uzyskiwania dostępu do strony, która wymaga uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="1753b-180">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="1753b-181">Gdy użytkownik przesyła formularz na stronie logowania `AccountController` `Login` nosi nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="1753b-181">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="1753b-182">`Login` Wywołania akcji `PasswordSignInAsync` na `_signInManager` obiektu (udostępniane `AccountController` przez wstrzykiwanie zależności).</span><span class="sxs-lookup"><span data-stu-id="1753b-182">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="1753b-183">Podstawa (`Controller` lub `PageModel`) klasy ujawnia `User` właściwości.</span><span class="sxs-lookup"><span data-stu-id="1753b-183">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="1753b-184">Na przykład `User.Claims` mogą być wyliczane w celu podejmowania decyzji dotyczących autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="1753b-184">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="1753b-185">Wyloguj się</span><span class="sxs-lookup"><span data-stu-id="1753b-185">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1753b-186">**Wyloguj** wywołana `LogoutModel.OnPost` akcji.</span><span class="sxs-lookup"><span data-stu-id="1753b-186">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="1753b-187">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) czyści oświadczenia użytkownika przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="1753b-187">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span> <span data-ttu-id="1753b-188">Nie przekierowania po wywołaniu `SignOutAsync` lub użytkownik będzie **nie** wylogowanie.</span><span class="sxs-lookup"><span data-stu-id="1753b-188">Don't redirect after calling `SignOutAsync` or the user will **not** be signed out.</span></span>

<span data-ttu-id="1753b-189">Wpis jest określona w *Pages/Shared/_LoginPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1753b-189">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="1753b-190">Klikając **Wyloguj** link wywołania `LogOut` akcji.</span><span class="sxs-lookup"><span data-stu-id="1753b-190">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="1753b-191">Poprzedni kod wywołuje `_signInManager.SignOutAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="1753b-191">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="1753b-192">`SignOutAsync` Metoda czyści oświadczenia użytkownika przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="1753b-192">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="1753b-193">Testowanie tożsamości</span><span class="sxs-lookup"><span data-stu-id="1753b-193">Test Identity</span></span>

<span data-ttu-id="1753b-194">Domyślne szablony projektów sieci web zezwolić na dostęp anonimowy do strony głównej.</span><span class="sxs-lookup"><span data-stu-id="1753b-194">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="1753b-195">Aby sprawdzić tożsamość, należy dodać [ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) stronę Prywatność.</span><span class="sxs-lookup"><span data-stu-id="1753b-195">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

<span data-ttu-id="1753b-196">Jeśli użytkownik jest zalogowany, wyloguj się. Uruchom aplikację i wybierz **zachowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="1753b-196">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="1753b-197">Nastąpi przekierowanie do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="1753b-197">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="1753b-198">Zapoznaj się z tożsamości</span><span class="sxs-lookup"><span data-stu-id="1753b-198">Explore Identity</span></span>

<span data-ttu-id="1753b-199">Aby bardziej szczegółowo zapoznać się z tożsamości:</span><span class="sxs-lookup"><span data-stu-id="1753b-199">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="1753b-200">Tworzenie pełnej tożsamości interfejsu użytkownika źródła</span><span class="sxs-lookup"><span data-stu-id="1753b-200">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="1753b-201">Sprawdź źródła poszczególnych stron i krok po kroku debugera.</span><span class="sxs-lookup"><span data-stu-id="1753b-201">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="1753b-202">Składniki tożsamości</span><span class="sxs-lookup"><span data-stu-id="1753b-202">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="1753b-203">Wszystkie tożsamości zależne pakiety NuGet są objęte [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1753b-203">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="1753b-204">Pakiet podstawowego dla tożsamości jest [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="1753b-204">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="1753b-205">Ten pakiet zawiera podstawowy zestaw interfejsów dla produktu ASP.NET Core Identity i jest dołączony przez `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="1753b-205">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="1753b-206">Migrowanie tożsamości platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1753b-206">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="1753b-207">Aby uzyskać więcej informacji oraz wskazówki dotyczące migrowania istniejących sklepu tożsamości, zobacz [migracji uwierzytelnianie i tożsamość](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="1753b-207">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="1753b-208">Ustawianie siły hasła</span><span class="sxs-lookup"><span data-stu-id="1753b-208">Setting password strength</span></span>

<span data-ttu-id="1753b-209">Zobacz [konfiguracji](#pw) przykład określająca wymagania dotyczące minimalnych hasła.</span><span class="sxs-lookup"><span data-stu-id="1753b-209">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1753b-210">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="1753b-210">Next Steps</span></span>

* [<span data-ttu-id="1753b-211">Konfigurowanie tożsamości</span><span class="sxs-lookup"><span data-stu-id="1753b-211">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
