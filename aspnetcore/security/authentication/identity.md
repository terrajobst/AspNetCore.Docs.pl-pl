---
title: Wprowadzenie do tożsamości na ASP.NET Core
author: rick-anderson
description: Użyj tożsamości z aplikacją ASP.NET Core. Dowiedz się, jak ustawiać wymagania dotyczące haseł (RequireDigit, RequiredLength, RequiredUniqueChars itd.).
ms.author: riande
ms.date: 03/26/2019
uid: security/authentication/identity
ms.openlocfilehash: 979681cfc196aca9fb5097583d99a086e1c597ba
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082454"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="8a826-104">Wprowadzenie do tożsamości na ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a826-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="8a826-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8a826-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8a826-106">ASP.NET Core Identity to system członkostwa, który dodaje funkcje logowania do ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8a826-106">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="8a826-107">Użytkownicy mogą utworzyć konto z informacjami logowania przechowywanymi w tożsamości lub korzystać z zewnętrznego dostawcy logowania.</span><span class="sxs-lookup"><span data-stu-id="8a826-107">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="8a826-108">Obsługiwane zewnętrzne dostawcy logowania obejmują [serwis Facebook, Google, konto Microsoft i Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="8a826-108">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="8a826-109">Tożsamość można skonfigurować przy użyciu bazy danych SQL Server do przechowywania nazw użytkowników, haseł i danych profilu.</span><span class="sxs-lookup"><span data-stu-id="8a826-109">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="8a826-110">Alternatywnie można użyć innego magazynu trwałego, na przykład Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="8a826-110">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="8a826-111">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([jak pobrać)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="8a826-111">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="8a826-112">W tym temacie dowiesz się, jak używać tożsamości do rejestrowania, logowania i wylogowywania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8a826-112">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="8a826-113">Aby uzyskać bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji korzystających z tożsamości, zobacz sekcję następne kroki na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="8a826-113">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="8a826-114">AddDefaultIdentity i AddIdentity</span><span class="sxs-lookup"><span data-stu-id="8a826-114">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="8a826-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) został wprowadzony w ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="8a826-115">[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="8a826-116">Wywołanie `AddDefaultIdentity` jest podobne do wywołania następujących:</span><span class="sxs-lookup"><span data-stu-id="8a826-116">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* [<span data-ttu-id="8a826-117">AddIdentity</span><span class="sxs-lookup"><span data-stu-id="8a826-117">AddIdentity</span></span>](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [<span data-ttu-id="8a826-118">AddDefaultUI</span><span class="sxs-lookup"><span data-stu-id="8a826-118">AddDefaultUI</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [<span data-ttu-id="8a826-119">AddDefaultTokenProviders</span><span class="sxs-lookup"><span data-stu-id="8a826-119">AddDefaultTokenProviders</span></span>](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

<span data-ttu-id="8a826-120">Aby uzyskać więcej informacji, zobacz [Źródło AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="8a826-120">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="8a826-121">Tworzenie aplikacji sieci Web z uwierzytelnianiem</span><span class="sxs-lookup"><span data-stu-id="8a826-121">Create a Web app with authentication</span></span>

<span data-ttu-id="8a826-122">Utwórz projekt aplikacji sieci Web ASP.NET Core przy użyciu poszczególnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="8a826-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8a826-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8a826-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8a826-124">Wybierz pozycję **plik** > **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="8a826-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="8a826-125">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8a826-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="8a826-126">Nazwij projekt **WebApp1** tak, aby miał tę samą przestrzeń nazw co pobieranie projektu.</span><span class="sxs-lookup"><span data-stu-id="8a826-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="8a826-127">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="8a826-127">Click **OK**.</span></span>
* <span data-ttu-id="8a826-128">Wybierz **aplikację sieci Web**ASP.NET Core, a następnie wybierz pozycję **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="8a826-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="8a826-129">Wybierz pozycję **indywidualne konta użytkowników** , a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="8a826-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8a826-130">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8a826-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="8a826-131">Wygenerowany projekt zapewnia [ASP.NET Core tożsamość](xref:security/authentication/identity) jako [bibliotekę klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="8a826-131">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="8a826-132">Biblioteka klas Razor tożsamość ujawnia punkty końcowe z `Identity` obszarem.</span><span class="sxs-lookup"><span data-stu-id="8a826-132">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="8a826-133">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8a826-133">For example:</span></span>

* <span data-ttu-id="8a826-134">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="8a826-134">/Identity/Account/Login</span></span>
* <span data-ttu-id="8a826-135">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="8a826-135">/Identity/Account/Logout</span></span>
* <span data-ttu-id="8a826-136">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="8a826-136">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="8a826-137">Zastosuj migracje</span><span class="sxs-lookup"><span data-stu-id="8a826-137">Apply migrations</span></span>

<span data-ttu-id="8a826-138">Zastosuj migracje, aby zainicjować SFTP bazę danych.</span><span class="sxs-lookup"><span data-stu-id="8a826-138">Apply the migrations to initialise the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8a826-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8a826-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8a826-140">Uruchom następujące polecenie w konsoli Menedżera pakietów (PMC):</span><span class="sxs-lookup"><span data-stu-id="8a826-140">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8a826-141">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8a826-141">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="8a826-142">Testuj rejestr i logowanie</span><span class="sxs-lookup"><span data-stu-id="8a826-142">Test Register and Login</span></span>

<span data-ttu-id="8a826-143">Uruchom aplikację i zarejestruj użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8a826-143">Run the app and register a user.</span></span> <span data-ttu-id="8a826-144">W zależności od rozmiaru ekranu może być konieczne wybranie przycisku przełącznika nawigacji, aby wyświetlić linki **rejestru** i **logowania** .</span><span class="sxs-lookup"><span data-stu-id="8a826-144">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="8a826-145">Konfigurowanie usług tożsamości</span><span class="sxs-lookup"><span data-stu-id="8a826-145">Configure Identity services</span></span>

<span data-ttu-id="8a826-146">Usługi są dodawane do `ConfigureServices`programu.</span><span class="sxs-lookup"><span data-stu-id="8a826-146">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="8a826-147">Typowym wzorcem jest wywoływanie wszystkich `Add{Service}` metod, a następnie wywoływanie `services.Configure{Service}` wszystkich metod.</span><span class="sxs-lookup"><span data-stu-id="8a826-147">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="8a826-148">Poprzedni kod konfiguruje tożsamość z domyślnymi wartościami opcji.</span><span class="sxs-lookup"><span data-stu-id="8a826-148">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="8a826-149">Usługi są udostępniane aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8a826-149">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="8a826-150">Tożsamość jest włączona przez wywołanie [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="8a826-150">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="8a826-151">`UseAuthentication`dodaje [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) uwierzytelniania do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="8a826-151">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="8a826-152">Usługi są udostępniane aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8a826-152">Services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="8a826-153">Tożsamość jest włączona dla aplikacji przez wywołanie `UseAuthentication` `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="8a826-153">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="8a826-154">`UseAuthentication`dodaje [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) uwierzytelniania do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="8a826-154">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="8a826-155">Te usługi są udostępniane aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8a826-155">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="8a826-156">Tożsamość jest włączona dla aplikacji przez wywołanie `UseIdentity` `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="8a826-156">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="8a826-157">`UseIdentity`dodaje [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) uwierzytelniania opartego na plikach cookie do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="8a826-157">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

<span data-ttu-id="8a826-158">Aby uzyskać więcej informacji, zobacz [IdentityOptions klasy](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) i [uruchamiania aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="8a826-158">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="8a826-159">Rejestrowanie, logowanie i wylogowywanie szkieletu</span><span class="sxs-lookup"><span data-stu-id="8a826-159">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="8a826-160">Postępuj zgodnie z informacjami o [tożsamości szkieletowej w projekcie Razor z instrukcjami autoryzacji](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) w celu wygenerowania kodu pokazanego w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="8a826-160">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8a826-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8a826-161">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8a826-162">Dodaj pliki rejestru, logowania i wylogowywania.</span><span class="sxs-lookup"><span data-stu-id="8a826-162">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8a826-163">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8a826-163">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8a826-164">Jeśli utworzono projekt o nazwie **WebApp1**, uruchom następujące polecenia.</span><span class="sxs-lookup"><span data-stu-id="8a826-164">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="8a826-165">W przeciwnym razie użyj prawidłowej przestrzeni nazw `ApplicationDbContext`dla:</span><span class="sxs-lookup"><span data-stu-id="8a826-165">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="8a826-166">Program PowerShell używa średnika jako separatora poleceń.</span><span class="sxs-lookup"><span data-stu-id="8a826-166">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="8a826-167">W przypadku korzystania z programu PowerShell wpisz średniki na liście plików lub Umieść listę plików w podwójnym cudzysłowie, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="8a826-167">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="8a826-168">Badaj rejestr</span><span class="sxs-lookup"><span data-stu-id="8a826-168">Examine Register</span></span>

::: moniker range=">= aspnetcore-2.1"

   <span data-ttu-id="8a826-169">Gdy użytkownik kliknie łącze **zarejestruj** , `RegisterModel.OnPostAsync` zostanie wywołana akcja.</span><span class="sxs-lookup"><span data-stu-id="8a826-169">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="8a826-170">Użytkownik jest tworzony przez wartość [IsAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) dla `_userManager` obiektu.</span><span class="sxs-lookup"><span data-stu-id="8a826-170">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="8a826-171">`_userManager`jest dostarczany przez wstrzyknięcie zależności):</span><span class="sxs-lookup"><span data-stu-id="8a826-171">`_userManager` is provided by dependency injection):</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="8a826-172">Po kliknięciu przez użytkownika linku `Register` Register zostanie wywołana akcja w. `AccountController`</span><span class="sxs-lookup"><span data-stu-id="8a826-172">When a user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="8a826-173">Akcja tworzy użytkownika przez `_userManager` wywołanie `AccountController` obiektu (przekazanego przez iniekcję zależności): `CreateAsync` `Register`</span><span class="sxs-lookup"><span data-stu-id="8a826-173">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   <span data-ttu-id="8a826-174">Jeśli użytkownik został utworzony pomyślnie, użytkownik jest zalogowany przez wywołanie `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="8a826-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="8a826-175">**Uwaga:** Zobacz [potwierdzenie konta](xref:security/authentication/accconfirm#prevent-login-at-registration) , aby zapobiec natychmiastowemu logowaniu przy rejestracji.</span><span class="sxs-lookup"><span data-stu-id="8a826-175">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="8a826-176">Zaloguj się</span><span class="sxs-lookup"><span data-stu-id="8a826-176">Log in</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8a826-177">Formularz logowania jest wyświetlany, gdy:</span><span class="sxs-lookup"><span data-stu-id="8a826-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="8a826-178">Wybrano łącze **Zaloguj** .</span><span class="sxs-lookup"><span data-stu-id="8a826-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="8a826-179">Użytkownik próbuje uzyskać dostęp do strony z ograniczeniami, do której nie ma uprawnień dostępu **lub** kiedy nie został uwierzytelniony przez system.</span><span class="sxs-lookup"><span data-stu-id="8a826-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="8a826-180">Po przesłaniu `OnPostAsync` formularza na stronie logowania zostanie wywołana akcja.</span><span class="sxs-lookup"><span data-stu-id="8a826-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="8a826-181">`PasswordSignInAsync`jest wywoływana dla `_signInManager` obiektu (dostarczone przez iniekcję zależności).</span><span class="sxs-lookup"><span data-stu-id="8a826-181">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

   [!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

   <span data-ttu-id="8a826-182">Klasa bazowa `Controller` `User` uwidacznia właściwość, do której można uzyskać dostęp za pomocą metod kontrolera.</span><span class="sxs-lookup"><span data-stu-id="8a826-182">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="8a826-183">Na przykład można wyliczyć `User.Claims` i podjąć decyzje dotyczące autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="8a826-183">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="8a826-184">Aby uzyskać więcej informacji, zobacz <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="8a826-184">For more information, see <xref:security/authorization/introduction>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="8a826-185">Formularz logowania jest wyświetlany, gdy użytkownik wybierze link **Zaloguj** lub przekierowywać podczas uzyskiwania dostępu do strony wymagającej uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="8a826-185">The Login form is displayed when users select the **Log in** link or are redirected when accessing a page that requires authentication.</span></span> <span data-ttu-id="8a826-186">Gdy użytkownik prześle formularz na stronie logowania, `AccountController` `Login` akcja jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8a826-186">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

<span data-ttu-id="8a826-187">Akcja wywołuje `PasswordSignInAsync` obiekt(`AccountController` określony przez iniekcję zależności). `_signInManager` `Login`</span><span class="sxs-lookup"><span data-stu-id="8a826-187">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

<span data-ttu-id="8a826-188">Klasa bazowa (`Controller` lub `PageModel`) uwidacznia `User` właściwość.</span><span class="sxs-lookup"><span data-stu-id="8a826-188">The base (`Controller` or `PageModel`) class exposes a `User` property.</span></span> <span data-ttu-id="8a826-189">Na przykład `User.Claims` można wyliczyć w celu podejmowania decyzji dotyczących autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="8a826-189">For example, `User.Claims` can be enumerated to make authorization decisions.</span></span>

::: moniker-end

### <a name="log-out"></a><span data-ttu-id="8a826-190">Wyloguj się</span><span class="sxs-lookup"><span data-stu-id="8a826-190">Log out</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8a826-191">Link **Wyloguj** wywołuje `LogoutModel.OnPost` akcję.</span><span class="sxs-lookup"><span data-stu-id="8a826-191">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="8a826-192">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) czyści oświadczenia użytkownika przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8a826-192">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="8a826-193">Wpis jest określony na *stronie/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8a826-193">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   <span data-ttu-id="8a826-194">Kliknięcie linku **Wyloguj** wywołuje `LogOut` akcję.</span><span class="sxs-lookup"><span data-stu-id="8a826-194">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="8a826-195">Poprzedni kod wywołuje `_signInManager.SignOutAsync` metodę.</span><span class="sxs-lookup"><span data-stu-id="8a826-195">The preceding code calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="8a826-196">`SignOutAsync` Metoda czyści oświadczenia użytkownika przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8a826-196">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

::: moniker-end

## <a name="test-identity"></a><span data-ttu-id="8a826-197">Testuj tożsamość</span><span class="sxs-lookup"><span data-stu-id="8a826-197">Test Identity</span></span>

<span data-ttu-id="8a826-198">Domyślne szablony projektu sieci Web umożliwiają anonimowy dostęp do stron głównych.</span><span class="sxs-lookup"><span data-stu-id="8a826-198">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="8a826-199">Aby przetestować tożsamość, Dodaj [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) stronę prywatność.</span><span class="sxs-lookup"><span data-stu-id="8a826-199">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=6)]

<span data-ttu-id="8a826-200">Jeśli użytkownik jest zalogowany, Wyloguj się. Uruchom aplikację i wybierz łącze **prywatność** .</span><span class="sxs-lookup"><span data-stu-id="8a826-200">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="8a826-201">Nastąpi przekierowanie do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="8a826-201">You are redirected to the login page.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a><span data-ttu-id="8a826-202">Eksploruj tożsamość</span><span class="sxs-lookup"><span data-stu-id="8a826-202">Explore Identity</span></span>

<span data-ttu-id="8a826-203">Aby poznać tożsamość w bardziej szczegółowy sposób:</span><span class="sxs-lookup"><span data-stu-id="8a826-203">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="8a826-204">Utwórz źródło interfejsu użytkownika pełnej tożsamości</span><span class="sxs-lookup"><span data-stu-id="8a826-204">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="8a826-205">Przejrzyj Źródło poszczególnych stron i przechodzenie przez debuger.</span><span class="sxs-lookup"><span data-stu-id="8a826-205">Examine the source of each page and step through the debugger.</span></span>

::: moniker-end

## <a name="identity-components"></a><span data-ttu-id="8a826-206">Składniki tożsamości</span><span class="sxs-lookup"><span data-stu-id="8a826-206">Identity Components</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8a826-207">Wszystkie pakiety NuGet zależne od tożsamości są zawarte w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="8a826-207">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="8a826-208">Podstawowym pakietem tożsamości jest [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="8a826-208">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="8a826-209">Ten pakiet zawiera podstawowy zestaw interfejsów dla ASP.NET Core Identity i jest dołączony `Microsoft.AspNetCore.Identity.EntityFrameworkCore`do programu.</span><span class="sxs-lookup"><span data-stu-id="8a826-209">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="8a826-210">Migrowanie do ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="8a826-210">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="8a826-211">Aby uzyskać więcej informacji i wskazówek dotyczących migrowania istniejącego magazynu tożsamości, zobacz [Migrowanie uwierzytelniania i tożsamości](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="8a826-211">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="8a826-212">Ustawianie siły hasła</span><span class="sxs-lookup"><span data-stu-id="8a826-212">Setting password strength</span></span>

<span data-ttu-id="8a826-213">Zobacz [Konfiguracja](#pw) dla przykładu, który ustawia minimalne wymagania dotyczące hasła.</span><span class="sxs-lookup"><span data-stu-id="8a826-213">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a826-214">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="8a826-214">Next Steps</span></span>

* [<span data-ttu-id="8a826-215">Konfigurowanie tożsamości</span><span class="sxs-lookup"><span data-stu-id="8a826-215">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
