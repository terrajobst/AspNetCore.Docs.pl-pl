---
title: Wprowadzenie do tożsamości programu ASP.NET Core
author: rick-anderson
description: Tożsamość za pomocą aplikacji ASP.NET Core. Zawiera wymagania dotyczące hasła ustawienie (RequireDigit, RequiredLength, RequiredUniqueChars i inne).
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: 50ddb96000e6a3f9e1762e9bb3e1f215f20d4356
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095642"
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="29e6f-104">Wprowadzenie do tożsamości programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="29e6f-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="29e6f-105">Przez [autorem jest Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Galloway'em Jon [Erik Reitan](https://github.com/Erikre), i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="29e6f-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="29e6f-106">Tożsamość platformy ASP.NET Core jest systemu członkostwa, co pozwala na dodawanie funkcji logowania do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="29e6f-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="29e6f-107">Użytkownicy mogą tworzyć konta usługi i zaloguj się przy użyciu nazwy użytkownika i hasło lub można użyć dostawcy logowania zewnętrznego, takich jak Facebook, Google, Microsoft Account, Twitter lub inne osoby.</span><span class="sxs-lookup"><span data-stu-id="29e6f-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="29e6f-108">Można skonfigurować tożsamości platformy ASP.NET Core używać bazy danych programu SQL Server do przechowywania nazwy użytkownika, hasła i dane profilu.</span><span class="sxs-lookup"><span data-stu-id="29e6f-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="29e6f-109">Alternatywnie można użyć własnego magazynu trwałego na przykład Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="29e6f-109">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="29e6f-110">Ten dokument zawiera instrukcje dotyczące programu Visual Studio, a także uzyskać za pomocą interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="29e6f-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="29e6f-111">Wyświetlanie lub pobieranie przykładowego kodu.</span><span class="sxs-lookup"><span data-stu-id="29e6f-111">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="29e6f-112">(Jak pobrać)</span><span class="sxs-lookup"><span data-stu-id="29e6f-112">(How to download)</span></span>](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="29e6f-113">Przegląd tożsamości</span><span class="sxs-lookup"><span data-stu-id="29e6f-113">Overview of Identity</span></span>

<span data-ttu-id="29e6f-114">W tym temacie będziesz Dowiedz się, jak dodać funkcje, aby zarejestrować, zaloguj się za pomocą tożsamości platformy ASP.NET Core i wylogowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="29e6f-114">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="29e6f-115">Aby uzyskać bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji za pomocą tożsamości platformy ASP.NET Core zobacz sekcję następne kroki na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="29e6f-115">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1. <span data-ttu-id="29e6f-116">Tworzenie projektu aplikacji sieci Web programu ASP.NET Core z indywidualnymi kontami użytkowników.</span><span class="sxs-lookup"><span data-stu-id="29e6f-116">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

   # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="29e6f-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="29e6f-117">Visual Studio</span></span>](#tab/visual-studio)

   <span data-ttu-id="29e6f-118">W programie Visual Studio, wybierz **pliku** > **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="29e6f-118">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="29e6f-119">Wybierz **aplikacji sieci Web programu ASP.NET Core** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="29e6f-119">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

   ![Okno dialogowe nowego projektu](identity/_static/01-new-project.png)

   <span data-ttu-id="29e6f-121">Wybierz platformy ASP.NET Core **aplikacji sieci Web (Model-View-Controller)** dla platformy ASP.NET Core 2.x, a następnie wybierz **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="29e6f-121">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

   ![Okno dialogowe nowego projektu](identity/_static/02-new-project.png)

   <span data-ttu-id="29e6f-123">Zostanie wyświetlone okno dialogowe oferty opcje uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="29e6f-123">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="29e6f-124">Wybierz **indywidualne konta użytkowników** i kliknij przycisk **OK** aby powrócić do poprzedniego okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="29e6f-124">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

   ![Okno dialogowe nowego projektu](identity/_static/03-new-project-auth.png)

   <span data-ttu-id="29e6f-126">Wybieranie **indywidualne konta użytkowników** kieruje programu Visual Studio do tworzenia modeli, modele widoków, widoki, kontrolery i innych zasobów wymaganych do uwierzytelniania w ramach szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="29e6f-126">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

   # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="29e6f-127">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="29e6f-127">.NET Core CLI</span></span>](#tab/netcore-cli)

   <span data-ttu-id="29e6f-128">Jeśli używasz interfejsu wiersza polecenia platformy .NET Core, Utwórz nowy projekt za pomocą `dotnet new mvc --auth Individual`.</span><span class="sxs-lookup"><span data-stu-id="29e6f-128">If using the .NET Core CLI, create the new project using `dotnet new mvc --auth Individual`.</span></span> <span data-ttu-id="29e6f-129">To polecenie tworzy nowy projekt za pomocą tego samego kodu szablonu tożsamości, tworzonych w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="29e6f-129">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

   <span data-ttu-id="29e6f-130">Utworzono projekt zawiera `Microsoft.AspNetCore.Identity.EntityFrameworkCore` pakiet, który będzie się powtarzać, dane tożsamości i schematów do programu SQL Server przy użyciu [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="29e6f-130">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

   ---

2. <span data-ttu-id="29e6f-131">Konfigurowanie usługi zarządzania tożsamościami i Dodaj oprogramowanie pośredniczące w `Startup`.</span><span class="sxs-lookup"><span data-stu-id="29e6f-131">Configure Identity services and add middleware in `Startup`.</span></span>

   <span data-ttu-id="29e6f-132">Usługi tożsamości są dodawane do aplikacji w `ConfigureServices` method in Class metoda `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="29e6f-132">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="29e6f-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="29e6f-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   <span data-ttu-id="29e6f-134">Te usługi są dostępne dla aplikacji za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="29e6f-134">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="29e6f-135">Tożsamość jest włączone dla aplikacji, wywołując `UseAuthentication` w `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="29e6f-135">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="29e6f-136">`UseAuthentication` dodaje uwierzytelniania [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="29e6f-136">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="29e6f-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="29e6f-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   <span data-ttu-id="29e6f-138">Te usługi są dostępne dla aplikacji za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="29e6f-138">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

   <span data-ttu-id="29e6f-139">Tożsamość jest włączone dla aplikacji, wywołując `UseIdentity` w `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="29e6f-139">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="29e6f-140">`UseIdentity` dodaje na podstawie plików cookie uwierzytelniania [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="29e6f-140">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware/index) to the request pipeline.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   <span data-ttu-id="29e6f-141">Aby uzyskać więcej informacji na temat uruchamiania aplikacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="29e6f-141">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3. <span data-ttu-id="29e6f-142">Utwórz użytkownika.</span><span class="sxs-lookup"><span data-stu-id="29e6f-142">Create a user.</span></span>

   <span data-ttu-id="29e6f-143">Uruchom aplikację, a następnie kliknij polecenie **zarejestrować** łącza.</span><span class="sxs-lookup"><span data-stu-id="29e6f-143">Launch the application and then click on the **Register** link.</span></span>

   <span data-ttu-id="29e6f-144">Jeśli wykonujesz tę akcję po raz pierwszy, może być wymagane do uruchamiania migracji.</span><span class="sxs-lookup"><span data-stu-id="29e6f-144">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="29e6f-145">Aplikacja wyświetli monit o **zastosować migracje**.</span><span class="sxs-lookup"><span data-stu-id="29e6f-145">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="29e6f-146">Jeśli to konieczne, należy odświeżyć stronę.</span><span class="sxs-lookup"><span data-stu-id="29e6f-146">Refresh the page if needed.</span></span>

   ![Zastosuj migracje strony sieci Web](identity/_static/apply-migrations.png)

   <span data-ttu-id="29e6f-148">Alternatywnie można przetestować za pomocą tożsamości platformy ASP.NET Core z aplikacją, bez trwałego bazy danych przy użyciu bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="29e6f-148">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="29e6f-149">Aby użyć bazy danych w pamięci, należy dodać `Microsoft.EntityFrameworkCore.InMemory` pakietu z aplikacją i zmodyfikuj wywołanie aplikacji `AddDbContext` w `ConfigureServices` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="29e6f-149">To use an in-memory database, add the `Microsoft.EntityFrameworkCore.InMemory` package to your app and modify your app's call to `AddDbContext` in `ConfigureServices` as follows:</span></span>

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   <span data-ttu-id="29e6f-150">Kiedy użytkownik kliknie **zarejestrować** łącza, `Register` jest wywoływana Akcja `AccountController`.</span><span class="sxs-lookup"><span data-stu-id="29e6f-150">When the user clicks the **Register** link, the `Register` action is invoked on `AccountController`.</span></span> <span data-ttu-id="29e6f-151">`Register` Akcja powoduje utworzenie użytkownika, wywołując `CreateAsync` na `_userManager` obiektu (udostępniane `AccountController` przez wstrzykiwanie zależności):</span><span class="sxs-lookup"><span data-stu-id="29e6f-151">The `Register` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to `AccountController` by dependency injection):</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   <span data-ttu-id="29e6f-152">Jeśli użytkownik został pomyślnie utworzony, użytkownik jest zalogowany przez wywołanie `_signInManager.SignInAsync`.</span><span class="sxs-lookup"><span data-stu-id="29e6f-152">If the user was created successfully, the user is logged in by the call to `_signInManager.SignInAsync`.</span></span>

   <span data-ttu-id="29e6f-153">**Uwaga:** zobacz [konta potwierdzenia](xref:security/authentication/accconfirm#prevent-login-at-registration) kroki uniknąć natychmiastowego logowania podczas rejestracji.</span><span class="sxs-lookup"><span data-stu-id="29e6f-153">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>

4. <span data-ttu-id="29e6f-154">Zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="29e6f-154">Log in.</span></span>

   <span data-ttu-id="29e6f-155">Użytkownicy mogą się logować, klikając **Zaloguj** link u góry strony, lub może być nastąpi przejście do strony logowania, gdy próbują uzyskać dostęp do części witryny, która wymaga autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="29e6f-155">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="29e6f-156">Gdy użytkownik przesyła formularz na stronie logowania `AccountController` `Login` nosi nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="29e6f-156">When the user submits the form on the Login page, the `AccountController` `Login` action is called.</span></span>

   <span data-ttu-id="29e6f-157">`Login` Wywołania akcji `PasswordSignInAsync` na `_signInManager` obiektu (udostępniane `AccountController` przez wstrzykiwanie zależności).</span><span class="sxs-lookup"><span data-stu-id="29e6f-157">The `Login` action calls `PasswordSignInAsync` on the `_signInManager` object (provided to `AccountController` by dependency injection).</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   <span data-ttu-id="29e6f-158">Podstawa `Controller` klasy ujawnia `User` właściwość, której będziesz mieć dostęp z metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="29e6f-158">The base `Controller` class exposes a `User` property that you can access from controller methods.</span></span> <span data-ttu-id="29e6f-159">Na przykład, można wyliczyć `User.Claims` i podejmowania decyzji dotyczących autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="29e6f-159">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="29e6f-160">Aby uzyskać więcej informacji, zobacz [autoryzacji](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="29e6f-160">For more information, see [Authorization](xref:security/authorization/index).</span></span>

5. <span data-ttu-id="29e6f-161">Zaloguj.</span><span class="sxs-lookup"><span data-stu-id="29e6f-161">Log out.</span></span>

   <span data-ttu-id="29e6f-162">Klikając **Wyloguj** link wywołania `LogOut` akcji.</span><span class="sxs-lookup"><span data-stu-id="29e6f-162">Clicking the **Log out** link calls the `LogOut` action.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   <span data-ttu-id="29e6f-163">Powyższy kod powyżej wywołania `_signInManager.SignOutAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="29e6f-163">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="29e6f-164">`SignOutAsync` Metoda czyści oświadczenia użytkownika przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="29e6f-164">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>

<a name="pw"></a>
6. <span data-ttu-id="29e6f-165">Konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="29e6f-165">Configuration.</span></span>

   <span data-ttu-id="29e6f-166">Tożsamość ma niektóre zachowania domyślne, które mogą zostać zastąpione w klasie uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="29e6f-166">Identity has some default behaviors that can be overridden in the app's startup class.</span></span> <span data-ttu-id="29e6f-167">`IdentityOptions` nie należy skonfigurować, korzystając z zachowania domyślnego.</span><span class="sxs-lookup"><span data-stu-id="29e6f-167">`IdentityOptions` don't need to be configured when using the default behaviors.</span></span> <span data-ttu-id="29e6f-168">Poniższy kod ustawia kilka opcji siły hasła:</span><span class="sxs-lookup"><span data-stu-id="29e6f-168">The following code sets several password strength options:</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="29e6f-169">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="29e6f-169">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="29e6f-170">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="29e6f-170">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   <span data-ttu-id="29e6f-171">Aby uzyskać więcej informacji o sposobie konfigurowania tożsamości, zobacz [konfigurowania tożsamości](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="29e6f-171">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>

   <span data-ttu-id="29e6f-172">Można także skonfigurować typ danych klucza podstawowego, zobacz [konfigurowania tożsamości kluczy podstawowych danych typu](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="29e6f-172">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>

7. <span data-ttu-id="29e6f-173">Wyświetl bazy danych.</span><span class="sxs-lookup"><span data-stu-id="29e6f-173">View the database.</span></span>

   <span data-ttu-id="29e6f-174">Jeśli aplikacja używa bazy danych programu SQL Server (ustawienie domyślne dla Windows i dla użytkowników programu Visual Studio), możesz wyświetlić aplikacja utworzona w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="29e6f-174">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="29e6f-175">Możesz użyć **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="29e6f-175">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="29e6f-176">W programie Visual Studio, wybierz opcję **widoku** > **Eksplorator obiektów SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="29e6f-176">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="29e6f-177">Połączyć się z **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="29e6f-177">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="29e6f-178">Bazy danych z nazwą pasującą `aspnet-<name of your project>-<guid>` jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="29e6f-178">The database with a name matching `aspnet-<name of your project>-<guid>` is displayed.</span></span>

   ![Menu kontekstowe w tabeli bazy danych AspNetUsers](identity/_static/04-db.png)

   <span data-ttu-id="29e6f-180">Rozwiń bazę danych i jego **tabel**, kliknij prawym przyciskiem myszy **dbo. AspNetUsers** tabeli, a następnie wybierz pozycję **dane widoku**.</span><span class="sxs-lookup"><span data-stu-id="29e6f-180">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="29e6f-181">Sprawdź, czy działa tożsamości</span><span class="sxs-lookup"><span data-stu-id="29e6f-181">Verify Identity works</span></span>

    <span data-ttu-id="29e6f-182">Wartość domyślna *aplikacji sieci Web programu ASP.NET Core* szablon projektu umożliwia użytkownikom uzyskiwanie dostępu do żadnych działań w aplikacji bez konieczności do logowania.</span><span class="sxs-lookup"><span data-stu-id="29e6f-182">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="29e6f-183">Aby sprawdzić, czy produktu ASP.NET Identity działa, Dodaj`[Authorize]` atrybutu `About` akcji `Home` kontrolera.</span><span class="sxs-lookup"><span data-stu-id="29e6f-183">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="29e6f-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="29e6f-184">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="29e6f-185">Uruchom projekt za pomocą **Ctrl** + **F5** i przejdź do **o** strony.</span><span class="sxs-lookup"><span data-stu-id="29e6f-185">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="29e6f-186">Tylko uwierzytelnieni użytkownicy mogą uzyskiwać dostęp do **o** teraz strony ASP.NET przekieruje Cię do strony logowania, aby zalogować się lub zarejestruj.</span><span class="sxs-lookup"><span data-stu-id="29e6f-186">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="29e6f-187">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="29e6f-187">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="29e6f-188">Otwórz okno polecenia i przejdź do katalogu głównego projektu katalogu zawierającego `.csproj` pliku.</span><span class="sxs-lookup"><span data-stu-id="29e6f-188">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="29e6f-189">Uruchom [dotnet, uruchom](/dotnet/core/tools/dotnet-run) polecenia do uruchomienia aplikacji:</span><span class="sxs-lookup"><span data-stu-id="29e6f-189">Run the [dotnet run](/dotnet/core/tools/dotnet-run) command to run the app:</span></span>

    ```csharp
    dotnet run 
    ```

    <span data-ttu-id="29e6f-190">Przejdź do adresu URL określonego w danych wyjściowych [dotnet, uruchom](/dotnet/core/tools/dotnet-run) polecenia.</span><span class="sxs-lookup"><span data-stu-id="29e6f-190">Browse the URL specified in the output from the [dotnet run](/dotnet/core/tools/dotnet-run) command.</span></span> <span data-ttu-id="29e6f-191">Adres URL powinien wskazywać `localhost` za pomocą wygenerowanego numeru portu.</span><span class="sxs-lookup"><span data-stu-id="29e6f-191">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="29e6f-192">Przejdź do **o** strony.</span><span class="sxs-lookup"><span data-stu-id="29e6f-192">Navigate to the **About** page.</span></span> <span data-ttu-id="29e6f-193">Tylko uwierzytelnieni użytkownicy mogą uzyskiwać dostęp do **o** teraz strony ASP.NET przekieruje Cię do strony logowania, aby zalogować się lub zarejestruj.</span><span class="sxs-lookup"><span data-stu-id="29e6f-193">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="29e6f-194">Składniki tożsamości</span><span class="sxs-lookup"><span data-stu-id="29e6f-194">Identity Components</span></span>

<span data-ttu-id="29e6f-195">Zestaw odwołanie podstawowe dla systemu tożsamości jest `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="29e6f-195">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="29e6f-196">Ten pakiet zawiera podstawowy zestaw interfejsów dla produktu ASP.NET Core Identity i jest dołączony przez `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="29e6f-196">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="29e6f-197">Te zależności są niezbędne do używania systemu tożsamości w aplikacji platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="29e6f-197">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="29e6f-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Zawiera wymaganych typów tożsamości za pomocą platformy Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="29e6f-198">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="29e6f-199">`Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core jest technologii dostępu do danych zalecane przez firmę Microsoft dla relacyjnych baz danych, takich jak program SQL Server.</span><span class="sxs-lookup"><span data-stu-id="29e6f-199">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="29e6f-200">W przypadku testowania można użyć `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="29e6f-200">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="29e6f-201">`Microsoft.AspNetCore.Authentication.Cookies` -Oprogramowania pośredniczącego, które umożliwia aplikacji korzystanie z uwierzytelniania na podstawie plików cookie.</span><span class="sxs-lookup"><span data-stu-id="29e6f-201">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="29e6f-202">Migrowanie tożsamości platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="29e6f-202">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="29e6f-203">Dla dodatkowe informacje i wskazówki dotyczące migrowania istniejących tożsamości ze sklepu zobacz [migracji uwierzytelnianie i tożsamość](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="29e6f-203">For additional information and guidance on migrating your existing Identity store see [Migrate Authentication and Identity](xref:migration/identity).</span></span>

## <a name="setting-password-strength"></a><span data-ttu-id="29e6f-204">Ustawianie siły hasła</span><span class="sxs-lookup"><span data-stu-id="29e6f-204">Setting password strength</span></span>

<span data-ttu-id="29e6f-205">Zobacz [konfiguracji](#pw) przykład określająca wymagania dotyczące minimalnych hasła.</span><span class="sxs-lookup"><span data-stu-id="29e6f-205">See [Configuration](#pw) for a sample that sets the minimum password requirements.</span></span>

## <a name="next-steps"></a><span data-ttu-id="29e6f-206">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="29e6f-206">Next Steps</span></span>

* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:security/authentication/social/index>
* <xref:host-and-deploy/web-farm>
