---
title: Wprowadzenie do tożsamości na ASP.NET Core
author: rick-anderson
description: Użyj tożsamości z aplikacją ASP.NET Core. Dowiedz się, jak ustawiać wymagania dotyczące haseł (RequireDigit, RequiredLength, RequiredUniqueChars itd.).
ms.author: riande
ms.date: 12/05/2019
uid: security/authentication/identity
ms.openlocfilehash: c867b73a96fd081f6e2ca17fef561ac539c0a129
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880732"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="01ee1-104">Wprowadzenie do tożsamości na ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="01ee1-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="01ee1-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="01ee1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="01ee1-106">ASP.NET Core tożsamość:</span><span class="sxs-lookup"><span data-stu-id="01ee1-106">ASP.NET Core Identity:</span></span>

* <span data-ttu-id="01ee1-107">Jest interfejsem API, który obsługuje funkcje logowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="01ee1-107">Is an API that supports user interface (UI) login functionality.</span></span>
* <span data-ttu-id="01ee1-108">Zarządza użytkownikami, hasłami, danymi profilu, rolami, oświadczeniami, tokenami, potwierdzeniem wiadomości e-mail i innymi.</span><span class="sxs-lookup"><span data-stu-id="01ee1-108">Manages users, passwords, profile data, roles, claims, tokens, email confirmation, and more.</span></span>

<span data-ttu-id="01ee1-109">Użytkownicy mogą utworzyć konto z informacjami logowania przechowywanymi w tożsamości lub korzystać z zewnętrznego dostawcy logowania.</span><span class="sxs-lookup"><span data-stu-id="01ee1-109">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="01ee1-110">Obsługiwane zewnętrzne dostawcy logowania obejmują [serwis Facebook, Google, konto Microsoft i Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="01ee1-110">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="01ee1-111">[Kod źródłowy tożsamości](https://github.com/aspnet/AspNetCore/tree/master/src/Identity) jest dostępny w serwisie GitHub.</span><span class="sxs-lookup"><span data-stu-id="01ee1-111">The [Identity source code](https://github.com/aspnet/AspNetCore/tree/master/src/Identity) is available on GitHub.</span></span> <span data-ttu-id="01ee1-112">[Tożsamość szkieletu](xref:security/authentication/scaffold-identity) i wyświetlanie wygenerowanych plików w celu przejrzenia interakcji szablonu z tożsamością.</span><span class="sxs-lookup"><span data-stu-id="01ee1-112">[Scaffold Identity](xref:security/authentication/scaffold-identity) and view the generated files to review the template interaction with Identity.</span></span>

<span data-ttu-id="01ee1-113">Tożsamość jest zazwyczaj konfigurowana przy użyciu bazy danych SQL Server do przechowywania nazw użytkowników, haseł i danych profilu.</span><span class="sxs-lookup"><span data-stu-id="01ee1-113">Identity is typically configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="01ee1-114">Alternatywnie można użyć innego magazynu trwałego, na przykład Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="01ee1-114">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="01ee1-115">W tym temacie dowiesz się, jak używać tożsamości do rejestrowania, logowania i wylogowywania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="01ee1-115">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="01ee1-116">Aby uzyskać bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji korzystających z tożsamości, zobacz sekcję następne kroki na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="01ee1-116">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<span data-ttu-id="01ee1-117">[Platforma tożsamości firmy Microsoft](/azure/active-directory/develop/) to:</span><span class="sxs-lookup"><span data-stu-id="01ee1-117">[Microsoft identity platform](/azure/active-directory/develop/) is:</span></span>

* <span data-ttu-id="01ee1-118">Ewolucja platformy deweloperów Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="01ee1-118">An evolution of the Azure Active Directory (Azure AD) developer platform.</span></span>
* <span data-ttu-id="01ee1-119">Niepowiązane z tożsamością ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="01ee1-119">Unrelated to ASP.NET Core Identity.</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

<span data-ttu-id="01ee1-120">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([jak pobrać)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="01ee1-120">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="01ee1-121">Tworzenie aplikacji sieci Web z uwierzytelnianiem</span><span class="sxs-lookup"><span data-stu-id="01ee1-121">Create a Web app with authentication</span></span>

<span data-ttu-id="01ee1-122">Utwórz projekt aplikacji sieci Web ASP.NET Core przy użyciu poszczególnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="01ee1-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="01ee1-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01ee1-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="01ee1-124">Wybierz pozycję **plik** > **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="01ee1-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="01ee1-125">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="01ee1-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="01ee1-126">Nazwij projekt **WebApp1** tak, aby miał tę samą przestrzeń nazw co pobieranie projektu.</span><span class="sxs-lookup"><span data-stu-id="01ee1-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="01ee1-127">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="01ee1-127">Click **OK**.</span></span>
* <span data-ttu-id="01ee1-128">Wybierz **aplikację sieci Web**ASP.NET Core, a następnie wybierz pozycję **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="01ee1-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="01ee1-129">Wybierz pozycję **indywidualne konta użytkowników** , a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="01ee1-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="01ee1-130">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="01ee1-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="01ee1-131">Poprzednie polecenie tworzy aplikację sieci Web Razor przy użyciu oprogramowania SQLite.</span><span class="sxs-lookup"><span data-stu-id="01ee1-131">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="01ee1-132">Aby utworzyć aplikację sieci Web za pomocą LocalDB, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="01ee1-132">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="01ee1-133">Wygenerowany projekt zapewnia [ASP.NET Core tożsamość](xref:security/authentication/identity) jako [bibliotekę klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="01ee1-133">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="01ee1-134">Biblioteka klas Razor tożsamość ujawnia punkty końcowe z obszarem `Identity`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-134">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="01ee1-135">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="01ee1-135">For example:</span></span>

* <span data-ttu-id="01ee1-136">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="01ee1-136">/Identity/Account/Login</span></span>
* <span data-ttu-id="01ee1-137">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="01ee1-137">/Identity/Account/Logout</span></span>
* <span data-ttu-id="01ee1-138">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="01ee1-138">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="01ee1-139">Zastosuj migracje</span><span class="sxs-lookup"><span data-stu-id="01ee1-139">Apply migrations</span></span>

<span data-ttu-id="01ee1-140">Zastosuj migracje, aby zainicjować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="01ee1-140">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="01ee1-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01ee1-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="01ee1-142">Uruchom następujące polecenie w konsoli Menedżera pakietów (PMC):</span><span class="sxs-lookup"><span data-stu-id="01ee1-142">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="01ee1-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="01ee1-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="01ee1-144">W przypadku korzystania z oprogramowania SQLite migracja nie jest konieczna.</span><span class="sxs-lookup"><span data-stu-id="01ee1-144">Migrations are not necessary at this step when using SQLite.</span></span> <span data-ttu-id="01ee1-145">W przypadku LocalDB Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="01ee1-145">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="01ee1-146">Testuj rejestr i logowanie</span><span class="sxs-lookup"><span data-stu-id="01ee1-146">Test Register and Login</span></span>

<span data-ttu-id="01ee1-147">Uruchom aplikację i zarejestruj użytkownika.</span><span class="sxs-lookup"><span data-stu-id="01ee1-147">Run the app and register a user.</span></span> <span data-ttu-id="01ee1-148">W zależności od rozmiaru ekranu może być konieczne wybranie przycisku przełącznika nawigacji, aby wyświetlić linki **rejestru** i **logowania** .</span><span class="sxs-lookup"><span data-stu-id="01ee1-148">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="01ee1-149">Konfigurowanie usług tożsamości</span><span class="sxs-lookup"><span data-stu-id="01ee1-149">Configure Identity services</span></span>

<span data-ttu-id="01ee1-150">Usługi są dodawane w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-150">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="01ee1-151">Typowym wzorcem jest Wywołaj wszystkie metody `Add{Service}`, a następnie Wywołaj wszystkie metody `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-151">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

<span data-ttu-id="01ee1-152">Poprzedni wyróżniony kod konfiguruje tożsamość z domyślnymi wartościami opcji.</span><span class="sxs-lookup"><span data-stu-id="01ee1-152">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="01ee1-153">Usługi są udostępniane aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="01ee1-153">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="01ee1-154">Tożsamość jest włączona, wywołując <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span><span class="sxs-lookup"><span data-stu-id="01ee1-154">Identity is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="01ee1-155">`UseAuthentication` dodaje [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) uwierzytelniania do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="01ee1-155">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="01ee1-156">Aplikacja wygenerowana przez szablon nie korzysta z [autoryzacji](xref:security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="01ee1-156">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="01ee1-157">`app.UseAuthorization` jest dołączona, aby upewnić się, że została dodana w odpowiedniej kolejności, gdyby aplikacja mogła dodać autoryzację.</span><span class="sxs-lookup"><span data-stu-id="01ee1-157">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="01ee1-158">`UseRouting`, `UseAuthentication`, `UseAuthorization`i `UseEndpoints` muszą być wywoływane w kolejności pokazanej w powyższym kodzie.</span><span class="sxs-lookup"><span data-stu-id="01ee1-158">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="01ee1-159">Aby uzyskać więcej informacji na temat `IdentityOptions` i `Startup`, zobacz <xref:Microsoft.AspNetCore.Identity.IdentityOptions> i [uruchamiania aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="01ee1-159">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="01ee1-160">Rejestrowanie, logowanie i wylogowywanie szkieletu</span><span class="sxs-lookup"><span data-stu-id="01ee1-160">Scaffold Register, Login, and LogOut</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="01ee1-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01ee1-161">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="01ee1-162">Dodaj pliki rejestru, logowania i wylogowywania.</span><span class="sxs-lookup"><span data-stu-id="01ee1-162">Add the Register, Login, and LogOut files.</span></span> <span data-ttu-id="01ee1-163">Postępuj zgodnie z informacjami o [tożsamości szkieletowej w projekcie Razor z instrukcjami autoryzacji](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) w celu wygenerowania kodu pokazanego w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="01ee1-163">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="01ee1-164">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="01ee1-164">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="01ee1-165">Jeśli utworzono projekt o nazwie **WebApp1**, uruchom następujące polecenia.</span><span class="sxs-lookup"><span data-stu-id="01ee1-165">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="01ee1-166">W przeciwnym razie użyj prawidłowej przestrzeni nazw dla `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="01ee1-166">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="01ee1-167">Program PowerShell używa średnika jako separatora poleceń.</span><span class="sxs-lookup"><span data-stu-id="01ee1-167">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="01ee1-168">W przypadku korzystania z programu PowerShell wpisz średniki na liście plików lub Umieść listę plików w podwójnym cudzysłowie, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="01ee1-168">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="01ee1-169">Aby uzyskać więcej informacji na temat tworzenia szkieletów tożsamości, zobacz [tożsamość szkieletu w projekcie Razor z autoryzacją](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span><span class="sxs-lookup"><span data-stu-id="01ee1-169">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="01ee1-170">Badaj rejestr</span><span class="sxs-lookup"><span data-stu-id="01ee1-170">Examine Register</span></span>

<span data-ttu-id="01ee1-171">Gdy użytkownik kliknie łącze **zarejestruj** , zostanie wywołana akcja `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-171">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="01ee1-172">Użytkownik jest tworzony przez wartość [IsAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) na obiekcie `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-172">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="01ee1-173">`_userManager` jest zapewniona przez wstrzyknięcie zależności):</span><span class="sxs-lookup"><span data-stu-id="01ee1-173">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="01ee1-174">Jeśli użytkownik został utworzony pomyślnie, użytkownik jest zalogowany przez wywołanie do `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="01ee1-175">Zobacz [potwierdzenie konta](xref:security/authentication/accconfirm#prevent-login-at-registration) , aby zapobiec natychmiastowemu logowaniu przy rejestracji.</span><span class="sxs-lookup"><span data-stu-id="01ee1-175">See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="01ee1-176">Zaloguj się</span><span class="sxs-lookup"><span data-stu-id="01ee1-176">Log in</span></span>

<span data-ttu-id="01ee1-177">Formularz logowania jest wyświetlany, gdy:</span><span class="sxs-lookup"><span data-stu-id="01ee1-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="01ee1-178">Wybrano łącze **Zaloguj** .</span><span class="sxs-lookup"><span data-stu-id="01ee1-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="01ee1-179">Użytkownik próbuje uzyskać dostęp do strony z ograniczeniami, do której nie ma uprawnień dostępu **lub** kiedy nie został uwierzytelniony przez system.</span><span class="sxs-lookup"><span data-stu-id="01ee1-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="01ee1-180">Po przesłaniu formularza na stronie logowania zostanie wywołana akcja `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="01ee1-181">`PasswordSignInAsync` jest wywoływana na obiekcie `_signInManager` (dostarczony przez iniekcję zależności).</span><span class="sxs-lookup"><span data-stu-id="01ee1-181">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="01ee1-182">Klasa bazowa `Controller` uwidacznia Właściwość `User`, do której można uzyskać dostęp z metod kontrolera.</span><span class="sxs-lookup"><span data-stu-id="01ee1-182">The base `Controller` class exposes a `User` property that can be accessed from controller methods.</span></span> <span data-ttu-id="01ee1-183">Na przykład można wyliczyć `User.Claims` i podejmować decyzje dotyczące autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="01ee1-183">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="01ee1-184">Aby uzyskać więcej informacji, zobacz temat <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="01ee1-184">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="01ee1-185">Wyloguj się</span><span class="sxs-lookup"><span data-stu-id="01ee1-185">Log out</span></span>

<span data-ttu-id="01ee1-186">Link **Wyloguj** wywołuje akcję `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-186">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="01ee1-187">W powyższym kodzie kod `return RedirectToPage();` musi być przekierowaniem, aby przeglądarka wykonywała nowe żądanie, a tożsamość użytkownika zostanie zaktualizowana.</span><span class="sxs-lookup"><span data-stu-id="01ee1-187">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="01ee1-188">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) czyści oświadczenia użytkownika przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="01ee1-188">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="01ee1-189">Wpis jest określony na *stronie/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="01ee1-189">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="01ee1-190">Testuj tożsamość</span><span class="sxs-lookup"><span data-stu-id="01ee1-190">Test Identity</span></span>

<span data-ttu-id="01ee1-191">Domyślne szablony projektu sieci Web umożliwiają anonimowy dostęp do stron głównych.</span><span class="sxs-lookup"><span data-stu-id="01ee1-191">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="01ee1-192">Aby przetestować tożsamość, Dodaj [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span><span class="sxs-lookup"><span data-stu-id="01ee1-192">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="01ee1-193">Jeśli użytkownik jest zalogowany, Wyloguj się. Uruchom aplikację i wybierz łącze **prywatność** .</span><span class="sxs-lookup"><span data-stu-id="01ee1-193">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="01ee1-194">Nastąpi przekierowanie do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="01ee1-194">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="01ee1-195">Eksploruj tożsamość</span><span class="sxs-lookup"><span data-stu-id="01ee1-195">Explore Identity</span></span>

<span data-ttu-id="01ee1-196">Aby poznać tożsamość w bardziej szczegółowy sposób:</span><span class="sxs-lookup"><span data-stu-id="01ee1-196">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="01ee1-197">Utwórz źródło interfejsu użytkownika pełnej tożsamości</span><span class="sxs-lookup"><span data-stu-id="01ee1-197">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="01ee1-198">Przejrzyj Źródło poszczególnych stron i przechodzenie przez debuger.</span><span class="sxs-lookup"><span data-stu-id="01ee1-198">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="01ee1-199">Składniki tożsamości</span><span class="sxs-lookup"><span data-stu-id="01ee1-199">Identity Components</span></span>

<span data-ttu-id="01ee1-200">Wszystkie pakiety NuGet zależne od tożsamości są zawarte w [ASP.NET Core udostępnionej platformie](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span><span class="sxs-lookup"><span data-stu-id="01ee1-200">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="01ee1-201">Podstawowym pakietem tożsamości jest [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="01ee1-201">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="01ee1-202">Ten pakiet zawiera podstawowy zestaw interfejsów dla ASP.NET Core Identity i jest dołączany przez `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-202">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="01ee1-203">Migrowanie do ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="01ee1-203">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="01ee1-204">Aby uzyskać więcej informacji i wskazówek dotyczących migrowania istniejącego magazynu tożsamości, zobacz [Migrowanie uwierzytelniania i tożsamości](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="01ee1-204">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="01ee1-205">Ustawianie siły hasła</span><span class="sxs-lookup"><span data-stu-id="01ee1-205">Setting password strength</span></span>

<span data-ttu-id="01ee1-206">Zobacz [Konfiguracja](#pw) dla przykładu, który ustawia minimalne wymagania dotyczące hasła.</span><span class="sxs-lookup"><span data-stu-id="01ee1-206">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="01ee1-207">AddDefaultIdentity i AddIdentity</span><span class="sxs-lookup"><span data-stu-id="01ee1-207">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="01ee1-208"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> został wprowadzony w ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="01ee1-208"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="01ee1-209">Wywoływanie `AddDefaultIdentity` jest podobne do wywoływania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="01ee1-209">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="01ee1-210">Aby uzyskać więcej informacji, zobacz [Źródło AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="01ee1-210">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="01ee1-211">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="01ee1-211">Next Steps</span></span>

* [<span data-ttu-id="01ee1-212">Konfigurowanie tożsamości</span><span class="sxs-lookup"><span data-stu-id="01ee1-212">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="01ee1-213">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="01ee1-213">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="01ee1-214">ASP.NET Core Identity to system członkostwa, który dodaje funkcje logowania do ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="01ee1-214">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="01ee1-215">Użytkownicy mogą utworzyć konto z informacjami logowania przechowywanymi w tożsamości lub korzystać z zewnętrznego dostawcy logowania.</span><span class="sxs-lookup"><span data-stu-id="01ee1-215">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="01ee1-216">Obsługiwane zewnętrzne dostawcy logowania obejmują [serwis Facebook, Google, konto Microsoft i Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="01ee1-216">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="01ee1-217">Tożsamość można skonfigurować przy użyciu bazy danych SQL Server do przechowywania nazw użytkowników, haseł i danych profilu.</span><span class="sxs-lookup"><span data-stu-id="01ee1-217">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="01ee1-218">Alternatywnie można użyć innego magazynu trwałego, na przykład Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="01ee1-218">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="01ee1-219">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([jak pobrać)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="01ee1-219">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="01ee1-220">W tym temacie dowiesz się, jak używać tożsamości do rejestrowania, logowania i wylogowywania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="01ee1-220">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="01ee1-221">Aby uzyskać bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji korzystających z tożsamości, zobacz sekcję następne kroki na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="01ee1-221">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="01ee1-222">AddDefaultIdentity i AddIdentity</span><span class="sxs-lookup"><span data-stu-id="01ee1-222">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="01ee1-223"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> został wprowadzony w ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="01ee1-223"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="01ee1-224">Wywoływanie `AddDefaultIdentity` jest podobne do wywoływania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="01ee1-224">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="01ee1-225">Aby uzyskać więcej informacji, zobacz [Źródło AddDefaultIdentity](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="01ee1-225">See [AddDefaultIdentity source](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="01ee1-226">Tworzenie aplikacji sieci Web z uwierzytelnianiem</span><span class="sxs-lookup"><span data-stu-id="01ee1-226">Create a Web app with authentication</span></span>

<span data-ttu-id="01ee1-227">Utwórz projekt aplikacji sieci Web ASP.NET Core przy użyciu poszczególnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="01ee1-227">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="01ee1-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01ee1-228">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="01ee1-229">Wybierz pozycję **plik** > **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="01ee1-229">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="01ee1-230">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="01ee1-230">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="01ee1-231">Nazwij projekt **WebApp1** tak, aby miał tę samą przestrzeń nazw co pobieranie projektu.</span><span class="sxs-lookup"><span data-stu-id="01ee1-231">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="01ee1-232">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="01ee1-232">Click **OK**.</span></span>
* <span data-ttu-id="01ee1-233">Wybierz **aplikację sieci Web**ASP.NET Core, a następnie wybierz pozycję **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="01ee1-233">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="01ee1-234">Wybierz pozycję **indywidualne konta użytkowników** , a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="01ee1-234">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="01ee1-235">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="01ee1-235">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="01ee1-236">Wygenerowany projekt zapewnia [ASP.NET Core tożsamość](xref:security/authentication/identity) jako [bibliotekę klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="01ee1-236">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="01ee1-237">Biblioteka klas Razor tożsamość ujawnia punkty końcowe z obszarem `Identity`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-237">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="01ee1-238">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="01ee1-238">For example:</span></span>

* <span data-ttu-id="01ee1-239">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="01ee1-239">/Identity/Account/Login</span></span>
* <span data-ttu-id="01ee1-240">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="01ee1-240">/Identity/Account/Logout</span></span>
* <span data-ttu-id="01ee1-241">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="01ee1-241">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="01ee1-242">Zastosuj migracje</span><span class="sxs-lookup"><span data-stu-id="01ee1-242">Apply migrations</span></span>

<span data-ttu-id="01ee1-243">Zastosuj migracje, aby zainicjować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="01ee1-243">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="01ee1-244">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01ee1-244">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="01ee1-245">Uruchom następujące polecenie w konsoli Menedżera pakietów (PMC):</span><span class="sxs-lookup"><span data-stu-id="01ee1-245">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="01ee1-246">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="01ee1-246">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="01ee1-247">Testuj rejestr i logowanie</span><span class="sxs-lookup"><span data-stu-id="01ee1-247">Test Register and Login</span></span>

<span data-ttu-id="01ee1-248">Uruchom aplikację i zarejestruj użytkownika.</span><span class="sxs-lookup"><span data-stu-id="01ee1-248">Run the app and register a user.</span></span> <span data-ttu-id="01ee1-249">W zależności od rozmiaru ekranu może być konieczne wybranie przycisku przełącznika nawigacji, aby wyświetlić linki **rejestru** i **logowania** .</span><span class="sxs-lookup"><span data-stu-id="01ee1-249">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="01ee1-250">Konfigurowanie usług tożsamości</span><span class="sxs-lookup"><span data-stu-id="01ee1-250">Configure Identity services</span></span>

<span data-ttu-id="01ee1-251">Usługi są dodawane w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-251">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="01ee1-252">Typowym wzorcem jest Wywołaj wszystkie metody `Add{Service}`, a następnie Wywołaj wszystkie metody `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-252">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="01ee1-253">Poprzedni kod konfiguruje tożsamość z domyślnymi wartościami opcji.</span><span class="sxs-lookup"><span data-stu-id="01ee1-253">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="01ee1-254">Usługi są udostępniane aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="01ee1-254">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="01ee1-255">Tożsamość jest włączona przez wywołanie [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="01ee1-255">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="01ee1-256">`UseAuthentication` dodaje [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) uwierzytelniania do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="01ee1-256">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="01ee1-257">Aby uzyskać więcej informacji, zobacz [IdentityOptions klasy](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) i [uruchamiania aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="01ee1-257">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="01ee1-258">Rejestrowanie, logowanie i wylogowywanie szkieletu</span><span class="sxs-lookup"><span data-stu-id="01ee1-258">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="01ee1-259">Postępuj zgodnie z informacjami o [tożsamości szkieletowej w projekcie Razor z instrukcjami autoryzacji](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) w celu wygenerowania kodu pokazanego w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="01ee1-259">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="01ee1-260">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="01ee1-260">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="01ee1-261">Dodaj pliki rejestru, logowania i wylogowywania.</span><span class="sxs-lookup"><span data-stu-id="01ee1-261">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="01ee1-262">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="01ee1-262">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="01ee1-263">Jeśli utworzono projekt o nazwie **WebApp1**, uruchom następujące polecenia.</span><span class="sxs-lookup"><span data-stu-id="01ee1-263">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="01ee1-264">W przeciwnym razie użyj prawidłowej przestrzeni nazw dla `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="01ee1-264">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="01ee1-265">Program PowerShell używa średnika jako separatora poleceń.</span><span class="sxs-lookup"><span data-stu-id="01ee1-265">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="01ee1-266">W przypadku korzystania z programu PowerShell wpisz średniki na liście plików lub Umieść listę plików w podwójnym cudzysłowie, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="01ee1-266">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="01ee1-267">Badaj rejestr</span><span class="sxs-lookup"><span data-stu-id="01ee1-267">Examine Register</span></span>

<span data-ttu-id="01ee1-268">Gdy użytkownik kliknie łącze **zarejestruj** , zostanie wywołana akcja `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-268">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="01ee1-269">Użytkownik jest tworzony przez wartość [IsAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) na obiekcie `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-269">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="01ee1-270">`_userManager` jest zapewniona przez wstrzyknięcie zależności):</span><span class="sxs-lookup"><span data-stu-id="01ee1-270">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="01ee1-271">Jeśli użytkownik został utworzony pomyślnie, użytkownik jest zalogowany przez wywołanie do `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-271">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="01ee1-272">**Uwaga:** Zobacz [potwierdzenie konta](xref:security/authentication/accconfirm#prevent-login-at-registration) , aby zapobiec natychmiastowemu logowaniu przy rejestracji.</span><span class="sxs-lookup"><span data-stu-id="01ee1-272">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="01ee1-273">Zaloguj się</span><span class="sxs-lookup"><span data-stu-id="01ee1-273">Log in</span></span>

<span data-ttu-id="01ee1-274">Formularz logowania jest wyświetlany, gdy:</span><span class="sxs-lookup"><span data-stu-id="01ee1-274">The Login form is displayed when:</span></span>

* <span data-ttu-id="01ee1-275">Wybrano łącze **Zaloguj** .</span><span class="sxs-lookup"><span data-stu-id="01ee1-275">The **Log in** link is selected.</span></span>
* <span data-ttu-id="01ee1-276">Użytkownik próbuje uzyskać dostęp do strony z ograniczeniami, do której nie ma uprawnień dostępu **lub** kiedy nie został uwierzytelniony przez system.</span><span class="sxs-lookup"><span data-stu-id="01ee1-276">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="01ee1-277">Po przesłaniu formularza na stronie logowania zostanie wywołana akcja `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-277">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="01ee1-278">`PasswordSignInAsync` jest wywoływana na obiekcie `_signInManager` (dostarczony przez iniekcję zależności).</span><span class="sxs-lookup"><span data-stu-id="01ee1-278">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="01ee1-279">Klasa bazowa `Controller` uwidacznia Właściwość `User`, do której można uzyskać dostęp z metod kontrolera.</span><span class="sxs-lookup"><span data-stu-id="01ee1-279">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="01ee1-280">Na przykład można wyliczyć `User.Claims` i podejmować decyzje dotyczące autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="01ee1-280">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="01ee1-281">Aby uzyskać więcej informacji, zobacz temat <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="01ee1-281">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="01ee1-282">Wyloguj się</span><span class="sxs-lookup"><span data-stu-id="01ee1-282">Log out</span></span>

<span data-ttu-id="01ee1-283">Link **Wyloguj** wywołuje akcję `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-283">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="01ee1-284">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) czyści oświadczenia użytkownika przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="01ee1-284">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="01ee1-285">Wpis jest określony na *stronie/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="01ee1-285">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="01ee1-286">Testuj tożsamość</span><span class="sxs-lookup"><span data-stu-id="01ee1-286">Test Identity</span></span>

<span data-ttu-id="01ee1-287">Domyślne szablony projektu sieci Web umożliwiają anonimowy dostęp do stron głównych.</span><span class="sxs-lookup"><span data-stu-id="01ee1-287">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="01ee1-288">Aby przetestować tożsamość, Dodaj [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) do strony prywatność.</span><span class="sxs-lookup"><span data-stu-id="01ee1-288">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="01ee1-289">Jeśli użytkownik jest zalogowany, Wyloguj się. Uruchom aplikację i wybierz łącze **prywatność** .</span><span class="sxs-lookup"><span data-stu-id="01ee1-289">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="01ee1-290">Nastąpi przekierowanie do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="01ee1-290">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="01ee1-291">Eksploruj tożsamość</span><span class="sxs-lookup"><span data-stu-id="01ee1-291">Explore Identity</span></span>

<span data-ttu-id="01ee1-292">Aby poznać tożsamość w bardziej szczegółowy sposób:</span><span class="sxs-lookup"><span data-stu-id="01ee1-292">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="01ee1-293">Utwórz źródło interfejsu użytkownika pełnej tożsamości</span><span class="sxs-lookup"><span data-stu-id="01ee1-293">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="01ee1-294">Przejrzyj Źródło poszczególnych stron i przechodzenie przez debuger.</span><span class="sxs-lookup"><span data-stu-id="01ee1-294">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="01ee1-295">Składniki tożsamości</span><span class="sxs-lookup"><span data-stu-id="01ee1-295">Identity Components</span></span>

<span data-ttu-id="01ee1-296">Wszystkie pakiety NuGet zależne od tożsamości są zawarte w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="01ee1-296">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="01ee1-297">Podstawowym pakietem tożsamości jest [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="01ee1-297">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="01ee1-298">Ten pakiet zawiera podstawowy zestaw interfejsów dla ASP.NET Core Identity i jest dołączany przez `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="01ee1-298">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="01ee1-299">Migrowanie do ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="01ee1-299">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="01ee1-300">Aby uzyskać więcej informacji i wskazówek dotyczących migrowania istniejącego magazynu tożsamości, zobacz [Migrowanie uwierzytelniania i tożsamości](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="01ee1-300">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="01ee1-301">Ustawianie siły hasła</span><span class="sxs-lookup"><span data-stu-id="01ee1-301">Setting password strength</span></span>

<span data-ttu-id="01ee1-302">Zobacz [Konfiguracja](#pw) dla przykładu, który ustawia minimalne wymagania dotyczące hasła.</span><span class="sxs-lookup"><span data-stu-id="01ee1-302">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="01ee1-303">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="01ee1-303">Next Steps</span></span>

* [<span data-ttu-id="01ee1-304">Konfigurowanie tożsamości</span><span class="sxs-lookup"><span data-stu-id="01ee1-304">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
