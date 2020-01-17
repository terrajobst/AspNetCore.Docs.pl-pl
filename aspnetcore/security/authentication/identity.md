---
title: Wprowadzenie do tożsamości na ASP.NET Core
author: rick-anderson
description: Użyj tożsamości z aplikacją ASP.NET Core. Dowiedz się, jak ustawiać wymagania dotyczące haseł (RequireDigit, RequiredLength, RequiredUniqueChars itd.).
ms.author: riande
ms.date: 01/15/2020
uid: security/authentication/identity
ms.openlocfilehash: 98fee261a741a20eed181ca5b9a4ebb693deeb63
ms.sourcegitcommit: cbd30479f42cbb3385000ef834d9c7d021fd218d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2020
ms.locfileid: "76146514"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="a4215-104">Wprowadzenie do tożsamości na ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a4215-104">Introduction to Identity on ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a4215-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a4215-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a4215-106">ASP.NET Core tożsamość:</span><span class="sxs-lookup"><span data-stu-id="a4215-106">ASP.NET Core Identity:</span></span>

* <span data-ttu-id="a4215-107">Jest interfejsem API, który obsługuje funkcje logowania interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a4215-107">Is an API that supports user interface (UI) login functionality.</span></span>
* <span data-ttu-id="a4215-108">Zarządza użytkownikami, hasłami, danymi profilu, rolami, oświadczeniami, tokenami, potwierdzeniem wiadomości e-mail i innymi.</span><span class="sxs-lookup"><span data-stu-id="a4215-108">Manages users, passwords, profile data, roles, claims, tokens, email confirmation, and more.</span></span>

<span data-ttu-id="a4215-109">Użytkownicy mogą utworzyć konto z informacjami logowania przechowywanymi w tożsamości lub korzystać z zewnętrznego dostawcy logowania.</span><span class="sxs-lookup"><span data-stu-id="a4215-109">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="a4215-110">Obsługiwane zewnętrzne dostawcy logowania obejmują [serwis Facebook, Google, konto Microsoft i Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="a4215-110">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="a4215-111">[Kod źródłowy tożsamości](https://github.com/dotnet/AspNetCore/tree/master/src/Identity) jest dostępny w serwisie GitHub.</span><span class="sxs-lookup"><span data-stu-id="a4215-111">The [Identity source code](https://github.com/dotnet/AspNetCore/tree/master/src/Identity) is available on GitHub.</span></span> <span data-ttu-id="a4215-112">[Tożsamość szkieletu](xref:security/authentication/scaffold-identity) i wyświetlanie wygenerowanych plików w celu przejrzenia interakcji szablonu z tożsamością.</span><span class="sxs-lookup"><span data-stu-id="a4215-112">[Scaffold Identity](xref:security/authentication/scaffold-identity) and view the generated files to review the template interaction with Identity.</span></span>

<span data-ttu-id="a4215-113">Tożsamość jest zazwyczaj konfigurowana przy użyciu bazy danych SQL Server do przechowywania nazw użytkowników, haseł i danych profilu.</span><span class="sxs-lookup"><span data-stu-id="a4215-113">Identity is typically configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="a4215-114">Alternatywnie można użyć innego magazynu trwałego, na przykład Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="a4215-114">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="a4215-115">W tym temacie dowiesz się, jak używać tożsamości do rejestrowania, logowania i wylogowywania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a4215-115">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="a4215-116">Aby uzyskać bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji korzystających z tożsamości, zobacz sekcję następne kroki na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="a4215-116">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<span data-ttu-id="a4215-117">[Platforma tożsamości firmy Microsoft](/azure/active-directory/develop/) to:</span><span class="sxs-lookup"><span data-stu-id="a4215-117">[Microsoft identity platform](/azure/active-directory/develop/) is:</span></span>

* <span data-ttu-id="a4215-118">Ewolucja platformy deweloperów Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a4215-118">An evolution of the Azure Active Directory (Azure AD) developer platform.</span></span>
* <span data-ttu-id="a4215-119">Niepowiązane z tożsamością ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a4215-119">Unrelated to ASP.NET Core Identity.</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

<span data-ttu-id="a4215-120">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([jak pobrać)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="a4215-120">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<a name="adi"></a>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="a4215-121">Tworzenie aplikacji sieci Web z uwierzytelnianiem</span><span class="sxs-lookup"><span data-stu-id="a4215-121">Create a Web app with authentication</span></span>

<span data-ttu-id="a4215-122">Utwórz projekt aplikacji sieci Web ASP.NET Core przy użyciu poszczególnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="a4215-122">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4215-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4215-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a4215-124">Wybierz pozycję **plik** > **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="a4215-124">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="a4215-125">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a4215-125">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a4215-126">Nazwij projekt **WebApp1** tak, aby miał tę samą przestrzeń nazw co pobieranie projektu.</span><span class="sxs-lookup"><span data-stu-id="a4215-126">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="a4215-127">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a4215-127">Click **OK**.</span></span>
* <span data-ttu-id="a4215-128">Wybierz **aplikację sieci Web**ASP.NET Core, a następnie wybierz pozycję **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="a4215-128">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="a4215-129">Wybierz pozycję **indywidualne konta użytkowników** , a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a4215-129">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a4215-130">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a4215-130">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

<span data-ttu-id="a4215-131">Poprzednie polecenie tworzy aplikację sieci Web Razor przy użyciu oprogramowania SQLite.</span><span class="sxs-lookup"><span data-stu-id="a4215-131">The preceding command creates a Razor web app using SQLite.</span></span> <span data-ttu-id="a4215-132">Aby utworzyć aplikację sieci Web za pomocą LocalDB, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="a4215-132">To create the web app with LocalDB, run the following command:</span></span>

```dotnetcli
dotnet new webapp --auth Individual -uld -o WebApp1
```

---

<span data-ttu-id="a4215-133">Wygenerowany projekt zapewnia [ASP.NET Core tożsamość](xref:security/authentication/identity) jako [bibliotekę klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="a4215-133">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="a4215-134">Biblioteka klas Razor tożsamość ujawnia punkty końcowe z obszarem `Identity`.</span><span class="sxs-lookup"><span data-stu-id="a4215-134">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="a4215-135">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a4215-135">For example:</span></span>

* <span data-ttu-id="a4215-136">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="a4215-136">/Identity/Account/Login</span></span>
* <span data-ttu-id="a4215-137">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="a4215-137">/Identity/Account/Logout</span></span>
* <span data-ttu-id="a4215-138">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="a4215-138">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="a4215-139">Zastosuj migracje</span><span class="sxs-lookup"><span data-stu-id="a4215-139">Apply migrations</span></span>

<span data-ttu-id="a4215-140">Zastosuj migracje, aby zainicjować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="a4215-140">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4215-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4215-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a4215-142">Uruchom następujące polecenie w konsoli Menedżera pakietów (PMC):</span><span class="sxs-lookup"><span data-stu-id="a4215-142">Run the following command in the Package Manager Console (PMC):</span></span>

`PM> Update-Database`

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a4215-143">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a4215-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a4215-144">W przypadku korzystania z oprogramowania SQLite migracja nie jest konieczna.</span><span class="sxs-lookup"><span data-stu-id="a4215-144">Migrations are not necessary at this step when using SQLite.</span></span> <span data-ttu-id="a4215-145">W przypadku LocalDB Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="a4215-145">For LocalDB, run the following command:</span></span>

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="a4215-146">Testuj rejestr i logowanie</span><span class="sxs-lookup"><span data-stu-id="a4215-146">Test Register and Login</span></span>

<span data-ttu-id="a4215-147">Uruchom aplikację i zarejestruj użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a4215-147">Run the app and register a user.</span></span> <span data-ttu-id="a4215-148">W zależności od rozmiaru ekranu może być konieczne wybranie przycisku przełącznika nawigacji, aby wyświetlić linki **rejestru** i **logowania** .</span><span class="sxs-lookup"><span data-stu-id="a4215-148">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="a4215-149">Konfigurowanie usług tożsamości</span><span class="sxs-lookup"><span data-stu-id="a4215-149">Configure Identity services</span></span>

<span data-ttu-id="a4215-150">Usługi są dodawane w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a4215-150">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="a4215-151">Typowym wzorcem jest Wywołaj wszystkie metody `Add{Service}`, a następnie Wywołaj wszystkie metody `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="a4215-151">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configureservices&highlight=10-99)]

<span data-ttu-id="a4215-152">Poprzedni wyróżniony kod konfiguruje tożsamość z domyślnymi wartościami opcji.</span><span class="sxs-lookup"><span data-stu-id="a4215-152">The preceding highlighted code configures Identity with default option values.</span></span> <span data-ttu-id="a4215-153">Usługi są udostępniane aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a4215-153">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="a4215-154">Tożsamość jest włączona, wywołując <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span><span class="sxs-lookup"><span data-stu-id="a4215-154">Identity is enabled by calling <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>.</span></span> <span data-ttu-id="a4215-155">`UseAuthentication` dodaje [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) uwierzytelniania do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="a4215-155">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp3/Startup.cs?name=snippet_configure&highlight=19)]

<span data-ttu-id="a4215-156">Aplikacja wygenerowana przez szablon nie korzysta z [autoryzacji](xref:security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="a4215-156">The template-generated app doesn't use [authorization](xref:security/authorization/secure-data).</span></span> <span data-ttu-id="a4215-157">`app.UseAuthorization` jest dołączona, aby upewnić się, że została dodana w odpowiedniej kolejności, gdyby aplikacja mogła dodać autoryzację.</span><span class="sxs-lookup"><span data-stu-id="a4215-157">`app.UseAuthorization` is included to ensure it's added in the correct order should the app add authorization.</span></span> <span data-ttu-id="a4215-158">`UseRouting`, `UseAuthentication`, `UseAuthorization`i `UseEndpoints` muszą być wywoływane w kolejności pokazanej w powyższym kodzie.</span><span class="sxs-lookup"><span data-stu-id="a4215-158">`UseRouting`, `UseAuthentication`, `UseAuthorization`, and `UseEndpoints` must be called in the order shown in the preceding code.</span></span>

<span data-ttu-id="a4215-159">Aby uzyskać więcej informacji na temat `IdentityOptions` i `Startup`, zobacz <xref:Microsoft.AspNetCore.Identity.IdentityOptions> i [uruchamiania aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a4215-159">For more information on `IdentityOptions` and `Startup`, see <xref:Microsoft.AspNetCore.Identity.IdentityOptions> and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="a4215-160">Rejestrowanie, logowanie i wylogowywanie szkieletu</span><span class="sxs-lookup"><span data-stu-id="a4215-160">Scaffold Register, Login, and LogOut</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4215-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4215-161">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a4215-162">Dodaj pliki rejestru, logowania i wylogowywania.</span><span class="sxs-lookup"><span data-stu-id="a4215-162">Add the Register, Login, and LogOut files.</span></span> <span data-ttu-id="a4215-163">Postępuj zgodnie z informacjami o [tożsamości szkieletowej w projekcie Razor z instrukcjami autoryzacji](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) w celu wygenerowania kodu pokazanego w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="a4215-163">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a4215-164">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a4215-164">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a4215-165">Jeśli utworzono projekt o nazwie **WebApp1**, uruchom następujące polecenia.</span><span class="sxs-lookup"><span data-stu-id="a4215-165">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="a4215-166">W przeciwnym razie użyj prawidłowej przestrzeni nazw dla `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="a4215-166">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="a4215-167">Program PowerShell używa średnika jako separatora poleceń.</span><span class="sxs-lookup"><span data-stu-id="a4215-167">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="a4215-168">W przypadku korzystania z programu PowerShell wpisz średniki na liście plików lub Umieść listę plików w podwójnym cudzysłowie, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="a4215-168">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

<span data-ttu-id="a4215-169">Aby uzyskać więcej informacji na temat tworzenia szkieletów tożsamości, zobacz [tożsamość szkieletu w projekcie Razor z autoryzacją](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span><span class="sxs-lookup"><span data-stu-id="a4215-169">For more information on scaffolding Identity, see [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization).</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="a4215-170">Badaj rejestr</span><span class="sxs-lookup"><span data-stu-id="a4215-170">Examine Register</span></span>

<span data-ttu-id="a4215-171">Gdy użytkownik kliknie łącze **zarejestruj** , zostanie wywołana akcja `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="a4215-171">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="a4215-172">Użytkownik jest tworzony przez wartość [IsAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) na obiekcie `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="a4215-172">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="a4215-173">`_userManager` jest zapewniona przez wstrzyknięcie zależności):</span><span class="sxs-lookup"><span data-stu-id="a4215-173">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=9)]

<span data-ttu-id="a4215-174">Jeśli użytkownik został utworzony pomyślnie, użytkownik jest zalogowany przez wywołanie do `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="a4215-174">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="a4215-175">Zobacz [potwierdzenie konta](xref:security/authentication/accconfirm#prevent-login-at-registration) , aby zapobiec natychmiastowemu logowaniu przy rejestracji.</span><span class="sxs-lookup"><span data-stu-id="a4215-175">See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="a4215-176">Zaloguj się</span><span class="sxs-lookup"><span data-stu-id="a4215-176">Log in</span></span>

<span data-ttu-id="a4215-177">Formularz logowania jest wyświetlany, gdy:</span><span class="sxs-lookup"><span data-stu-id="a4215-177">The Login form is displayed when:</span></span>

* <span data-ttu-id="a4215-178">Wybrano łącze **Zaloguj** .</span><span class="sxs-lookup"><span data-stu-id="a4215-178">The **Log in** link is selected.</span></span>
* <span data-ttu-id="a4215-179">Użytkownik próbuje uzyskać dostęp do strony z ograniczeniami, do której nie ma uprawnień dostępu **lub** kiedy nie został uwierzytelniony przez system.</span><span class="sxs-lookup"><span data-stu-id="a4215-179">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="a4215-180">Po przesłaniu formularza na stronie logowania zostanie wywołana akcja `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="a4215-180">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="a4215-181">`PasswordSignInAsync` jest wywoływana na obiekcie `_signInManager` (dostarczony przez iniekcję zależności).</span><span class="sxs-lookup"><span data-stu-id="a4215-181">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="a4215-182">Klasa bazowa `Controller` uwidacznia Właściwość `User`, do której można uzyskać dostęp z metod kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a4215-182">The base `Controller` class exposes a `User` property that can be accessed from controller methods.</span></span> <span data-ttu-id="a4215-183">Na przykład można wyliczyć `User.Claims` i podejmować decyzje dotyczące autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="a4215-183">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="a4215-184">Aby uzyskać więcej informacji, zobacz temat <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="a4215-184">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="a4215-185">Wyloguj się</span><span class="sxs-lookup"><span data-stu-id="a4215-185">Log out</span></span>

<span data-ttu-id="a4215-186">Link **Wyloguj** wywołuje akcję `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="a4215-186">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp3/Areas/Identity/Pages/Account/Logout.cshtml.cs?highlight=36)]

<span data-ttu-id="a4215-187">W powyższym kodzie kod `return RedirectToPage();` musi być przekierowaniem, aby przeglądarka wykonywała nowe żądanie, a tożsamość użytkownika zostanie zaktualizowana.</span><span class="sxs-lookup"><span data-stu-id="a4215-187">In the preceding code, the code `return RedirectToPage();` needs to be a redirect so that the browser performs a new request and the identity for the user gets updated.</span></span>

<span data-ttu-id="a4215-188">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) czyści oświadczenia użytkownika przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="a4215-188">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="a4215-189">Wpis jest określony na *stronie/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a4215-189">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Shared/_LoginPartial.cshtml?highlight=15)]

## <a name="test-identity"></a><span data-ttu-id="a4215-190">Testuj tożsamość</span><span class="sxs-lookup"><span data-stu-id="a4215-190">Test Identity</span></span>

<span data-ttu-id="a4215-191">Domyślne szablony projektu sieci Web umożliwiają anonimowy dostęp do stron głównych.</span><span class="sxs-lookup"><span data-stu-id="a4215-191">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="a4215-192">Aby przetestować tożsamość, Dodaj [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span><span class="sxs-lookup"><span data-stu-id="a4215-192">To test Identity, add [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute):</span></span>

[!code-csharp[](identity/sample/WebApp3/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="a4215-193">Jeśli użytkownik jest zalogowany, Wyloguj się. Uruchom aplikację i wybierz łącze **prywatność** .</span><span class="sxs-lookup"><span data-stu-id="a4215-193">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="a4215-194">Nastąpi przekierowanie do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="a4215-194">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="a4215-195">Eksploruj tożsamość</span><span class="sxs-lookup"><span data-stu-id="a4215-195">Explore Identity</span></span>

<span data-ttu-id="a4215-196">Aby poznać tożsamość w bardziej szczegółowy sposób:</span><span class="sxs-lookup"><span data-stu-id="a4215-196">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="a4215-197">Utwórz źródło interfejsu użytkownika pełnej tożsamości</span><span class="sxs-lookup"><span data-stu-id="a4215-197">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="a4215-198">Przejrzyj Źródło poszczególnych stron i przechodzenie przez debuger.</span><span class="sxs-lookup"><span data-stu-id="a4215-198">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="a4215-199">Składniki tożsamości</span><span class="sxs-lookup"><span data-stu-id="a4215-199">Identity Components</span></span>

<span data-ttu-id="a4215-200">Wszystkie pakiety NuGet zależne od tożsamości są zawarte w [ASP.NET Core udostępnionej platformie](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span><span class="sxs-lookup"><span data-stu-id="a4215-200">All the Identity-dependent NuGet packages are included in the [ASP.NET Core shared framework](xref:aspnetcore-3.0#use-the-aspnet-core-shared-framework).</span></span>

<span data-ttu-id="a4215-201">Podstawowym pakietem tożsamości jest [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="a4215-201">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="a4215-202">Ten pakiet zawiera podstawowy zestaw interfejsów dla ASP.NET Core Identity i jest dołączany przez `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="a4215-202">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="a4215-203">Migrowanie do ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="a4215-203">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="a4215-204">Aby uzyskać więcej informacji i wskazówek dotyczących migrowania istniejącego magazynu tożsamości, zobacz [Migrowanie uwierzytelniania i tożsamości](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="a4215-204">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="a4215-205">Ustawianie siły hasła</span><span class="sxs-lookup"><span data-stu-id="a4215-205">Setting password strength</span></span>

<span data-ttu-id="a4215-206">Zobacz [Konfiguracja](#pw) dla przykładu, który ustawia minimalne wymagania dotyczące hasła.</span><span class="sxs-lookup"><span data-stu-id="a4215-206">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="a4215-207">AddDefaultIdentity i AddIdentity</span><span class="sxs-lookup"><span data-stu-id="a4215-207">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="a4215-208"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> został wprowadzony w ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="a4215-208"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="a4215-209">Wywoływanie `AddDefaultIdentity` jest podobne do wywoływania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="a4215-209">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="a4215-210">Aby uzyskać więcej informacji, zobacz [Źródło AddDefaultIdentity](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="a4215-210">See [AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="prevent-publish-of-static-identity-assets"></a><span data-ttu-id="a4215-211">Zapobiegaj publikowaniu zasobów tożsamości statycznej</span><span class="sxs-lookup"><span data-stu-id="a4215-211">Prevent publish of static Identity assets</span></span>

<span data-ttu-id="a4215-212">Aby zapobiec publikowaniu statycznych zasobów tożsamości (arkuszy stylów i plików JavaScript dla interfejsu użytkownika tożsamości) do katalogu głównego sieci Web, Dodaj następującą właściwość `ResolveStaticWebAssetsInputsDependsOn` i obiekt docelowy `RemoveIdentityAssets` do pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="a4215-212">To prevent publishing static Identity assets (stylesheets and JavaScript files for Identity UI) to the web root, add the following `ResolveStaticWebAssetsInputsDependsOn` property and `RemoveIdentityAssets` target to the app's project file:</span></span>

```xml
<PropertyGroup>
  <ResolveStaticWebAssetsInputsDependsOn>RemoveIdentityAssets</ResolveStaticWebAssetsInputsDependsOn>
</PropertyGroup>

<Target Name="RemoveIdentityAssets">
  <ItemGroup>
    <StaticWebAsset Remove="@(StaticWebAsset)" Condition="%(SourceId) == 'Microsoft.AspNetCore.Identity.UI'" />
  </ItemGroup>
</Target>
```

## <a name="next-steps"></a><span data-ttu-id="a4215-213">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="a4215-213">Next Steps</span></span>

* <span data-ttu-id="a4215-214">Zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/5131) w usłudze GitHub, aby uzyskać informacje dotyczące konfigurowania tożsamości przy użyciu oprogramowania SQLite.</span><span class="sxs-lookup"><span data-stu-id="a4215-214">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* [<span data-ttu-id="a4215-215">Konfigurowanie tożsamości</span><span class="sxs-lookup"><span data-stu-id="a4215-215">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a4215-216">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a4215-216">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a4215-217">ASP.NET Core Identity to system członkostwa, który dodaje funkcje logowania do ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a4215-217">ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps.</span></span> <span data-ttu-id="a4215-218">Użytkownicy mogą utworzyć konto z informacjami logowania przechowywanymi w tożsamości lub korzystać z zewnętrznego dostawcy logowania.</span><span class="sxs-lookup"><span data-stu-id="a4215-218">Users can create an account with the login information stored in Identity or they can use an external login provider.</span></span> <span data-ttu-id="a4215-219">Obsługiwane zewnętrzne dostawcy logowania obejmują [serwis Facebook, Google, konto Microsoft i Twitter](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="a4215-219">Supported external login providers include [Facebook, Google, Microsoft Account, and Twitter](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="a4215-220">Tożsamość można skonfigurować przy użyciu bazy danych SQL Server do przechowywania nazw użytkowników, haseł i danych profilu.</span><span class="sxs-lookup"><span data-stu-id="a4215-220">Identity can be configured using a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="a4215-221">Alternatywnie można użyć innego magazynu trwałego, na przykład Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="a4215-221">Alternatively, another persistent store can be used, for example, Azure Table Storage.</span></span>

<span data-ttu-id="a4215-222">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([jak pobrać)](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="a4215-222">[View or download the sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) ([how to download)](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="a4215-223">W tym temacie dowiesz się, jak używać tożsamości do rejestrowania, logowania i wylogowywania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a4215-223">In this topic, you learn how to use Identity to register, log in, and log out a user.</span></span> <span data-ttu-id="a4215-224">Aby uzyskać bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji korzystających z tożsamości, zobacz sekcję następne kroki na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="a4215-224">For more detailed instructions about creating apps that use Identity, see the Next Steps section at the end of this article.</span></span>

<a name="adi"></a>

## <a name="adddefaultidentity-and-addidentity"></a><span data-ttu-id="a4215-225">AddDefaultIdentity i AddIdentity</span><span class="sxs-lookup"><span data-stu-id="a4215-225">AddDefaultIdentity and AddIdentity</span></span>

<span data-ttu-id="a4215-226"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> został wprowadzony w ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="a4215-226"><xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionUIExtensions.AddDefaultIdentity*> was introduced in ASP.NET Core 2.1.</span></span> <span data-ttu-id="a4215-227">Wywoływanie `AddDefaultIdentity` jest podobne do wywoływania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="a4215-227">Calling `AddDefaultIdentity` is similar to calling the following:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentity*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI*>
* <xref:Microsoft.AspNetCore.Identity.IdentityBuilderExtensions.AddDefaultTokenProviders*>

<span data-ttu-id="a4215-228">Aby uzyskać więcej informacji, zobacz [Źródło AddDefaultIdentity](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) .</span><span class="sxs-lookup"><span data-stu-id="a4215-228">See [AddDefaultIdentity source](https://github.com/dotnet/AspNetCore/blob/release/3.0/src/Identity/UI/src/IdentityServiceCollectionUIExtensions.cs#L47-L63) for more information.</span></span>

## <a name="create-a-web-app-with-authentication"></a><span data-ttu-id="a4215-229">Tworzenie aplikacji sieci Web z uwierzytelnianiem</span><span class="sxs-lookup"><span data-stu-id="a4215-229">Create a Web app with authentication</span></span>

<span data-ttu-id="a4215-230">Utwórz projekt aplikacji sieci Web ASP.NET Core przy użyciu poszczególnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="a4215-230">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4215-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4215-231">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a4215-232">Wybierz pozycję **plik** > **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="a4215-232">Select **File** > **New** > **Project**.</span></span>
* <span data-ttu-id="a4215-233">Wybierz **aplikacji sieci Web platformy ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a4215-233">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a4215-234">Nazwij projekt **WebApp1** tak, aby miał tę samą przestrzeń nazw co pobieranie projektu.</span><span class="sxs-lookup"><span data-stu-id="a4215-234">Name the project **WebApp1** to have the same namespace as the project download.</span></span> <span data-ttu-id="a4215-235">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a4215-235">Click **OK**.</span></span>
* <span data-ttu-id="a4215-236">Wybierz **aplikację sieci Web**ASP.NET Core, a następnie wybierz pozycję **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="a4215-236">Select an ASP.NET Core **Web Application**, then select **Change Authentication**.</span></span>
* <span data-ttu-id="a4215-237">Wybierz pozycję **indywidualne konta użytkowników** , a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="a4215-237">Select **Individual User Accounts** and click **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a4215-238">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a4215-238">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp --auth Individual -o WebApp1
```

---

<span data-ttu-id="a4215-239">Wygenerowany projekt zapewnia [ASP.NET Core tożsamość](xref:security/authentication/identity) jako [bibliotekę klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="a4215-239">The generated project provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="a4215-240">Biblioteka klas Razor tożsamość ujawnia punkty końcowe z obszarem `Identity`.</span><span class="sxs-lookup"><span data-stu-id="a4215-240">The Identity Razor Class Library exposes endpoints with the `Identity` area.</span></span> <span data-ttu-id="a4215-241">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a4215-241">For example:</span></span>

* <span data-ttu-id="a4215-242">/Identity/Account/Login</span><span class="sxs-lookup"><span data-stu-id="a4215-242">/Identity/Account/Login</span></span>
* <span data-ttu-id="a4215-243">/Identity/Account/Logout</span><span class="sxs-lookup"><span data-stu-id="a4215-243">/Identity/Account/Logout</span></span>
* <span data-ttu-id="a4215-244">/Identity/Account/Manage</span><span class="sxs-lookup"><span data-stu-id="a4215-244">/Identity/Account/Manage</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="a4215-245">Zastosuj migracje</span><span class="sxs-lookup"><span data-stu-id="a4215-245">Apply migrations</span></span>

<span data-ttu-id="a4215-246">Zastosuj migracje, aby zainicjować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="a4215-246">Apply the migrations to initialize the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4215-247">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4215-247">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a4215-248">Uruchom następujące polecenie w konsoli Menedżera pakietów (PMC):</span><span class="sxs-lookup"><span data-stu-id="a4215-248">Run the following command in the Package Manager Console (PMC):</span></span>

```PM> Update-Database```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a4215-249">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a4215-249">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef database update
```

---

### <a name="test-register-and-login"></a><span data-ttu-id="a4215-250">Testuj rejestr i logowanie</span><span class="sxs-lookup"><span data-stu-id="a4215-250">Test Register and Login</span></span>

<span data-ttu-id="a4215-251">Uruchom aplikację i zarejestruj użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a4215-251">Run the app and register a user.</span></span> <span data-ttu-id="a4215-252">W zależności od rozmiaru ekranu może być konieczne wybranie przycisku przełącznika nawigacji, aby wyświetlić linki **rejestru** i **logowania** .</span><span class="sxs-lookup"><span data-stu-id="a4215-252">Depending on your screen size, you might need to select the navigation toggle button to see the **Register** and **Login** links.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>

### <a name="configure-identity-services"></a><span data-ttu-id="a4215-253">Konfigurowanie usług tożsamości</span><span class="sxs-lookup"><span data-stu-id="a4215-253">Configure Identity services</span></span>

<span data-ttu-id="a4215-254">Usługi są dodawane w `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a4215-254">Services are added in `ConfigureServices`.</span></span> <span data-ttu-id="a4215-255">Typowym wzorcem jest Wywołaj wszystkie metody `Add{Service}`, a następnie Wywołaj wszystkie metody `services.Configure{Service}`.</span><span class="sxs-lookup"><span data-stu-id="a4215-255">The typical pattern is to call all the `Add{Service}` methods, and then call all the `services.Configure{Service}` methods.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configureservices)]

<span data-ttu-id="a4215-256">Poprzedni kod konfiguruje tożsamość z domyślnymi wartościami opcji.</span><span class="sxs-lookup"><span data-stu-id="a4215-256">The preceding code configures Identity with default option values.</span></span> <span data-ttu-id="a4215-257">Usługi są udostępniane aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a4215-257">Services are made available to the app through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="a4215-258">Tożsamość jest włączona przez wywołanie [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span><span class="sxs-lookup"><span data-stu-id="a4215-258">Identity is enabled by calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_).</span></span> <span data-ttu-id="a4215-259">`UseAuthentication` dodaje [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) uwierzytelniania do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="a4215-259">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

[!code-csharp[](identity/sample/WebApp1/Startup.cs?name=snippet_configure&highlight=18)]

<span data-ttu-id="a4215-260">Aby uzyskać więcej informacji, zobacz [IdentityOptions klasy](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) i [uruchamiania aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a4215-260">For more information, see the [IdentityOptions Class](/dotnet/api/microsoft.aspnetcore.identity.identityoptions) and [Application Startup](xref:fundamentals/startup).</span></span>

## <a name="scaffold-register-login-and-logout"></a><span data-ttu-id="a4215-261">Rejestrowanie, logowanie i wylogowywanie szkieletu</span><span class="sxs-lookup"><span data-stu-id="a4215-261">Scaffold Register, Login, and LogOut</span></span>

<span data-ttu-id="a4215-262">Postępuj zgodnie z informacjami o [tożsamości szkieletowej w projekcie Razor z instrukcjami autoryzacji](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) w celu wygenerowania kodu pokazanego w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="a4215-262">Follow the [Scaffold identity into a Razor project with authorization](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization) instructions to generate the code shown in this section.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a4215-263">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4215-263">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a4215-264">Dodaj pliki rejestru, logowania i wylogowywania.</span><span class="sxs-lookup"><span data-stu-id="a4215-264">Add the Register, Login, and LogOut files.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a4215-265">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a4215-265">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a4215-266">Jeśli utworzono projekt o nazwie **WebApp1**, uruchom następujące polecenia.</span><span class="sxs-lookup"><span data-stu-id="a4215-266">If you created the project with name **WebApp1**, run the following commands.</span></span> <span data-ttu-id="a4215-267">W przeciwnym razie użyj prawidłowej przestrzeni nazw dla `ApplicationDbContext`:</span><span class="sxs-lookup"><span data-stu-id="a4215-267">Otherwise, use the correct namespace for the `ApplicationDbContext`:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="a4215-268">Program PowerShell używa średnika jako separatora poleceń.</span><span class="sxs-lookup"><span data-stu-id="a4215-268">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="a4215-269">W przypadku korzystania z programu PowerShell wpisz średniki na liście plików lub Umieść listę plików w podwójnym cudzysłowie, jak pokazano w powyższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="a4215-269">When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.</span></span>

---

### <a name="examine-register"></a><span data-ttu-id="a4215-270">Badaj rejestr</span><span class="sxs-lookup"><span data-stu-id="a4215-270">Examine Register</span></span>

<span data-ttu-id="a4215-271">Gdy użytkownik kliknie łącze **zarejestruj** , zostanie wywołana akcja `RegisterModel.OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="a4215-271">When a user clicks the **Register** link, the `RegisterModel.OnPostAsync` action is invoked.</span></span> <span data-ttu-id="a4215-272">Użytkownik jest tworzony przez wartość [IsAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) na obiekcie `_userManager`.</span><span class="sxs-lookup"><span data-stu-id="a4215-272">The user is created by [CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_) on the `_userManager` object.</span></span> <span data-ttu-id="a4215-273">`_userManager` jest zapewniona przez wstrzyknięcie zależności):</span><span class="sxs-lookup"><span data-stu-id="a4215-273">`_userManager` is provided by dependency injection):</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=7)]

<span data-ttu-id="a4215-274">Jeśli użytkownik został utworzony pomyślnie, użytkownik jest zalogowany przez wywołanie do `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="a4215-274">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

<span data-ttu-id="a4215-275">**Uwaga:** Zobacz [potwierdzenie konta](xref:security/authentication/accconfirm#prevent-login-at-registration) , aby zapobiec natychmiastowemu logowaniu przy rejestracji.</span><span class="sxs-lookup"><span data-stu-id="a4215-275">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

### <a name="log-in"></a><span data-ttu-id="a4215-276">Zaloguj się</span><span class="sxs-lookup"><span data-stu-id="a4215-276">Log in</span></span>

<span data-ttu-id="a4215-277">Formularz logowania jest wyświetlany, gdy:</span><span class="sxs-lookup"><span data-stu-id="a4215-277">The Login form is displayed when:</span></span>

* <span data-ttu-id="a4215-278">Wybrano łącze **Zaloguj** .</span><span class="sxs-lookup"><span data-stu-id="a4215-278">The **Log in** link is selected.</span></span>
* <span data-ttu-id="a4215-279">Użytkownik próbuje uzyskać dostęp do strony z ograniczeniami, do której nie ma uprawnień dostępu **lub** kiedy nie został uwierzytelniony przez system.</span><span class="sxs-lookup"><span data-stu-id="a4215-279">A user attempts to access a restricted page that they aren't authorized to access **or** when they haven't been authenticated by the system.</span></span>

<span data-ttu-id="a4215-280">Po przesłaniu formularza na stronie logowania zostanie wywołana akcja `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="a4215-280">When the form on the Login page is submitted, the `OnPostAsync` action is called.</span></span> <span data-ttu-id="a4215-281">`PasswordSignInAsync` jest wywoływana na obiekcie `_signInManager` (dostarczony przez iniekcję zależności).</span><span class="sxs-lookup"><span data-stu-id="a4215-281">`PasswordSignInAsync` is called on the `_signInManager` object (provided by dependency injection).</span></span>

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Login.cshtml.cs?name=snippet&highlight=10-11)]

<span data-ttu-id="a4215-282">Klasa bazowa `Controller` uwidacznia Właściwość `User`, do której można uzyskać dostęp z metod kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a4215-282">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="a4215-283">Na przykład można wyliczyć `User.Claims` i podejmować decyzje dotyczące autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="a4215-283">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="a4215-284">Aby uzyskać więcej informacji, zobacz temat <xref:security/authorization/introduction>.</span><span class="sxs-lookup"><span data-stu-id="a4215-284">For more information, see <xref:security/authorization/introduction>.</span></span>

### <a name="log-out"></a><span data-ttu-id="a4215-285">Wyloguj się</span><span class="sxs-lookup"><span data-stu-id="a4215-285">Log out</span></span>

<span data-ttu-id="a4215-286">Link **Wyloguj** wywołuje akcję `LogoutModel.OnPost`.</span><span class="sxs-lookup"><span data-stu-id="a4215-286">The **Log out** link invokes the `LogoutModel.OnPost` action.</span></span> 

[!code-csharp[](identity/sample/WebApp1/Areas/Identity/Pages/Account/Logout.cshtml.cs)]

<span data-ttu-id="a4215-287">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) czyści oświadczenia użytkownika przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="a4215-287">[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) clears the user's claims stored in a cookie.</span></span>

<span data-ttu-id="a4215-288">Wpis jest określony na *stronie/Shared/_LoginPartial. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a4215-288">Post is specified in the *Pages/Shared/_LoginPartial.cshtml*:</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Shared/_LoginPartial.cshtml?highlight=16)]

## <a name="test-identity"></a><span data-ttu-id="a4215-289">Testuj tożsamość</span><span class="sxs-lookup"><span data-stu-id="a4215-289">Test Identity</span></span>

<span data-ttu-id="a4215-290">Domyślne szablony projektu sieci Web umożliwiają anonimowy dostęp do stron głównych.</span><span class="sxs-lookup"><span data-stu-id="a4215-290">The default web project templates allow anonymous access to the home pages.</span></span> <span data-ttu-id="a4215-291">Aby przetestować tożsamość, Dodaj [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) do strony prywatność.</span><span class="sxs-lookup"><span data-stu-id="a4215-291">To test Identity, add [`[Authorize]`](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) to the Privacy page.</span></span>

[!code-csharp[](identity/sample/WebApp1/Pages/Privacy.cshtml.cs?highlight=7)]

<span data-ttu-id="a4215-292">Jeśli użytkownik jest zalogowany, Wyloguj się. Uruchom aplikację i wybierz łącze **prywatność** .</span><span class="sxs-lookup"><span data-stu-id="a4215-292">If you are signed in, sign out. Run the app and select the **Privacy** link.</span></span> <span data-ttu-id="a4215-293">Nastąpi przekierowanie do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="a4215-293">You are redirected to the login page.</span></span>

### <a name="explore-identity"></a><span data-ttu-id="a4215-294">Eksploruj tożsamość</span><span class="sxs-lookup"><span data-stu-id="a4215-294">Explore Identity</span></span>

<span data-ttu-id="a4215-295">Aby poznać tożsamość w bardziej szczegółowy sposób:</span><span class="sxs-lookup"><span data-stu-id="a4215-295">To explore Identity in more detail:</span></span>

* [<span data-ttu-id="a4215-296">Utwórz źródło interfejsu użytkownika pełnej tożsamości</span><span class="sxs-lookup"><span data-stu-id="a4215-296">Create full identity UI source</span></span>](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* <span data-ttu-id="a4215-297">Przejrzyj Źródło poszczególnych stron i przechodzenie przez debuger.</span><span class="sxs-lookup"><span data-stu-id="a4215-297">Examine the source of each page and step through the debugger.</span></span>

## <a name="identity-components"></a><span data-ttu-id="a4215-298">Składniki tożsamości</span><span class="sxs-lookup"><span data-stu-id="a4215-298">Identity Components</span></span>

<span data-ttu-id="a4215-299">Wszystkie pakiety NuGet zależne od tożsamości są zawarte w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a4215-299">All the Identity dependent NuGet packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="a4215-300">Podstawowym pakietem tożsamości jest [Microsoft. AspNetCore. Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span><span class="sxs-lookup"><span data-stu-id="a4215-300">The primary package for Identity is [Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/).</span></span> <span data-ttu-id="a4215-301">Ten pakiet zawiera podstawowy zestaw interfejsów dla ASP.NET Core Identity i jest dołączany przez `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="a4215-301">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="a4215-302">Migrowanie do ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="a4215-302">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="a4215-303">Aby uzyskać więcej informacji i wskazówek dotyczących migrowania istniejącego magazynu tożsamości, zobacz [Migrowanie uwierzytelniania i tożsamości](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="a4215-303">For more information and guidance on migrating your existing Identity store, see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="a4215-304">Ustawianie siły hasła</span><span class="sxs-lookup"><span data-stu-id="a4215-304">Setting password strength</span></span>

<span data-ttu-id="a4215-305">Zobacz [Konfiguracja](#pw) dla przykładu, który ustawia minimalne wymagania dotyczące hasła.</span><span class="sxs-lookup"><span data-stu-id="a4215-305">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4215-306">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="a4215-306">Next Steps</span></span>

* <span data-ttu-id="a4215-307">Zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/5131) w usłudze GitHub, aby uzyskać informacje dotyczące konfigurowania tożsamości przy użyciu oprogramowania SQLite.</span><span class="sxs-lookup"><span data-stu-id="a4215-307">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/5131) for information on configuring Identity using SQLite.</span></span>
* [<span data-ttu-id="a4215-308">Konfigurowanie tożsamości</span><span class="sxs-lookup"><span data-stu-id="a4215-308">Configure Identity</span></span>](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>

::: moniker-end
