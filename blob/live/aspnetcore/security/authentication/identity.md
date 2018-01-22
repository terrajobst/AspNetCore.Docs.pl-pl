---
title: "Wprowadzenie do tożsamości na platformy ASP.NET Core"
author: rick-anderson
description: "Użyj tożsamości w aplikacji platformy ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 436a5ecfd126c9660591cd55efc1cc52b9493136
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="4371f-103">Wprowadzenie do tożsamości na platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4371f-103">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="4371f-104">Przez [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Dykstra Tomasz](https://github.com/tdykstra), Galloway Jan [Erik Reitan](https://github.com/Erikre), i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="4371f-104">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="4371f-105">Tożsamość platformy ASP.NET Core to system członkostwa, co pozwala na dodawanie funkcji logowania do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4371f-105">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="4371f-106">Użytkownicy mogą tworzyć konta i logowania przy użyciu nazwy użytkownika i hasło lub użyć dostawcy logowania zewnętrznego, takich jak Facebook, Google, Microsoft Account, Twitter lub innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="4371f-106">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="4371f-107">Można skonfigurować ASP.NET Identity Core używać bazy danych programu SQL Server do przechowywania nazwy użytkownika, hasła i danych profilu.</span><span class="sxs-lookup"><span data-stu-id="4371f-107">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="4371f-108">Alternatywnie można użyć własnych magazynu trwałego, na przykład magazynu tabel Azure.</span><span class="sxs-lookup"><span data-stu-id="4371f-108">Alternatively, you can use your own persistent store, for example, an Azure Table Storage.</span></span> <span data-ttu-id="4371f-109">Ten dokument zawiera instrukcje dla programu Visual Studio i przy użyciu interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="4371f-109">This document contains instructions for Visual Studio and for using the CLI.</span></span>

[<span data-ttu-id="4371f-110">Wyświetl lub pobrać przykładowy kod.</span><span class="sxs-lookup"><span data-stu-id="4371f-110">View or download the sample code.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [<span data-ttu-id="4371f-111">(Jak pobrać)</span><span class="sxs-lookup"><span data-stu-id="4371f-111">(How to download)</span></span>](https://docs.microsoft.com/en-us/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a><span data-ttu-id="4371f-112">Omówienie tożsamości</span><span class="sxs-lookup"><span data-stu-id="4371f-112">Overview of Identity</span></span>

<span data-ttu-id="4371f-113">W tym temacie będzie używanie ASP.NET Core Identity funkcje, aby zarejestrować, zaloguj się i wylogowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4371f-113">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="4371f-114">Bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji korzystających z platformy ASP.NET Identity Core zobacz sekcję następne kroki na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="4371f-114">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="4371f-115">Tworzenie projektu aplikacji sieci Web platformy ASP.NET Core z indywidualnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="4371f-115">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4371f-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4371f-116">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="4371f-117">W programie Visual Studio, wybierz **pliku** > **nowy** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="4371f-117">In Visual Studio, select **File** > **New** > **Project**.</span></span> <span data-ttu-id="4371f-118">Wybierz **aplikacji sieci Web platformy ASP.NET Core** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="4371f-118">Select **ASP.NET Core Web Application** and click **OK**.</span></span>

    ![Okno dialogowe nowego projektu](identity/_static/01-new-project.png)

    <span data-ttu-id="4371f-120">Wybierz platformy ASP.NET Core **aplikacji sieci Web (Model-View-Controller)** dla platformy ASP.NET Core 2.x, a następnie wybierz **Zmień uwierzytelnianie**.</span><span class="sxs-lookup"><span data-stu-id="4371f-120">Select an ASP.NET Core **Web Application (Model-View-Controller)** for ASP.NET Core 2.x, then select **Change Authentication**.</span></span>

    ![Okno dialogowe nowego projektu](identity/_static/02-new-project.png)

    <span data-ttu-id="4371f-122">Zostanie wyświetlone okno dialogowe wysyłania ofert opcje uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="4371f-122">A dialog appears offering authentication choices.</span></span> <span data-ttu-id="4371f-123">Wybierz **indywidualnych kont użytkowników** i kliknij przycisk **OK** aby powrócić do poprzedniego okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="4371f-123">Select **Individual User Accounts** and click **OK** to return to the previous dialog.</span></span>

    ![Okno dialogowe nowego projektu](identity/_static/03-new-project-auth.png)

    <span data-ttu-id="4371f-125">Wybieranie **indywidualnych kont użytkowników** kieruje Visual Studio do tworzenia modeli, ViewModels, widoki, kontrolery i inne zasoby wymagane do uwierzytelniania w ramach szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="4371f-125">Selecting **Individual User Accounts** directs Visual Studio to create Models, ViewModels, Views, Controllers, and other assets required for authentication as part of the project template.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4371f-126">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4371f-126">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="4371f-127">Jeśli używasz interfejsu wiersza polecenia platformy .NET Core, Utwórz nowy projekt za pomocą ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="4371f-127">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="4371f-128">To polecenie tworzy nowy projekt z tego samego kodu szablonu tożsamości tworzonych w Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4371f-128">This command creates a new project with the same Identity template code Visual Studio creates.</span></span>

    <span data-ttu-id="4371f-129">Utworzony projekt zawiera `Microsoft.AspNetCore.Identity.EntityFrameworkCore` pakiet, który będzie się powtarzać, dane tożsamości i schematu przy użyciu programu SQL Server [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="4371f-129">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which persists the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>

    ---

2.  <span data-ttu-id="4371f-130">Konfigurowanie usługi tożsamości i Dodaj oprogramowanie pośredniczące w `Startup`.</span><span class="sxs-lookup"><span data-stu-id="4371f-130">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="4371f-131">Usługi tożsamości są dodawane do aplikacji w `ConfigureServices` metoda `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="4371f-131">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4371f-132">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4371f-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    <span data-ttu-id="4371f-133">Te usługi są udostępniane dla aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4371f-133">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="4371f-134">Tożsamość jest włączone dla aplikacji przez wywołanie metody `UseAuthentication` w `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="4371f-134">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="4371f-135">`UseAuthentication`dodaje uwierzytelniania [oprogramowanie pośredniczące](xref:fundamentals/middleware) do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="4371f-135">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4371f-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4371f-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    <span data-ttu-id="4371f-137">Te usługi są udostępniane dla aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="4371f-137">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="4371f-138">Tożsamość jest włączone dla aplikacji przez wywołanie metody `UseIdentity` w `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="4371f-138">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="4371f-139">`UseIdentity`dodaje plik cookie uwierzytelniania [oprogramowanie pośredniczące](xref:fundamentals/middleware) do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="4371f-139">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    <span data-ttu-id="4371f-140">Aby uzyskać więcej informacji na temat uruchamiania aplikacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="4371f-140">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="4371f-141">Utwórz użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4371f-141">Create a user.</span></span>

    <span data-ttu-id="4371f-142">Uruchom aplikację, a następnie kliknij polecenie **zarejestrować** łącza.</span><span class="sxs-lookup"><span data-stu-id="4371f-142">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="4371f-143">Jeśli wykonujesz tę akcję po raz pierwszy, może być wymagany do uruchamiania migracji.</span><span class="sxs-lookup"><span data-stu-id="4371f-143">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="4371f-144">Aplikacja wyświetli monit o **zastosować migracje**.</span><span class="sxs-lookup"><span data-stu-id="4371f-144">The application prompts you to **Apply Migrations**.</span></span> <span data-ttu-id="4371f-145">Odśwież stronę, jeśli to konieczne.</span><span class="sxs-lookup"><span data-stu-id="4371f-145">Refresh the page if needed.</span></span>
    
    ![Zastosuj stronę sieci Web migracji](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="4371f-147">Alternatywnie można testować przy użyciu ASP.NET Core Identity z aplikacji bez trwałego bazy danych przy użyciu bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="4371f-147">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="4371f-148">Aby użyć bazy danych w pamięci, należy dodać ``Microsoft.EntityFrameworkCore.InMemory`` pakiet do aplikacji i zmodyfikuj wywołanie aplikacji ``AddDbContext`` w ``ConfigureServices`` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="4371f-148">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="4371f-149">Po kliknięciu przez użytkownika **zarejestrować** łącza, ``Register`` akcji jest wywoływana na ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="4371f-149">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="4371f-150">``Register`` Akcja tworzy użytkownika, wywołując `CreateAsync` na `_userManager` obiektu (podano ``AccountController`` przez iniekcji zależności):</span><span class="sxs-lookup"><span data-stu-id="4371f-150">The ``Register`` action creates the user by calling `CreateAsync` on the `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    <span data-ttu-id="4371f-151">Jeśli użytkownik został pomyślnie utworzony, użytkownik jest zalogowany przez wywołanie ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="4371f-151">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="4371f-152">**Uwaga:** zobacz [konta potwierdzenia](xref:security/authentication/accconfirm#prevent-login-at-registration) kroki zapobiec bezpośredniego logowania podczas rejestracji.</span><span class="sxs-lookup"><span data-stu-id="4371f-152">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="4371f-153">Zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="4371f-153">Log in.</span></span>
 
    <span data-ttu-id="4371f-154">Użytkownicy mogą rejestrować klikając **Zaloguj** łącze u góry strony, lub mogą zostać przesłane do strony logowania, gdy próbują uzyskać dostępu do części witryny, która wymaga autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="4371f-154">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="4371f-155">Gdy użytkownik przesyła formularz na stronie logowania ``AccountController`` ``Login`` nosi nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="4371f-155">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    <span data-ttu-id="4371f-156">``Login`` Wywołania akcji ``PasswordSignInAsync`` na ``_signInManager`` obiektu (podano ``AccountController`` przez iniekcji zależności).</span><span class="sxs-lookup"><span data-stu-id="4371f-156">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    <span data-ttu-id="4371f-157">Podstawowym ``Controller`` klasy ujawnia ``User`` właściwości, którego można korzystać z metod kontrolera.</span><span class="sxs-lookup"><span data-stu-id="4371f-157">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="4371f-158">Na przykład można wyliczyć `User.Claims` i podejmowania decyzji dotyczących autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="4371f-158">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="4371f-159">Aby uzyskać więcej informacji, zobacz [autoryzacji](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="4371f-159">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="4371f-160">Wyloguj się.</span><span class="sxs-lookup"><span data-stu-id="4371f-160">Log out.</span></span>
 
    <span data-ttu-id="4371f-161">Kliknięcie przycisku **Wyloguj się** link wywołania `LogOut` akcji.</span><span class="sxs-lookup"><span data-stu-id="4371f-161">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    <span data-ttu-id="4371f-162">Poprzedni kod powyżej wywołania `_signInManager.SignOutAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="4371f-162">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="4371f-163">`SignOutAsync` Metody czyści oświadczeń użytkownika przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="4371f-163">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="4371f-164">Konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="4371f-164">Configuration.</span></span>

    <span data-ttu-id="4371f-165">Tożsamość ma pewne domyślne zachowania przesłaniające w klasie uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4371f-165">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="4371f-166">Nie ma potrzeby konfigurowania ``IdentityOptions`` Jeśli korzystasz z domyślnego zachowania.</span><span class="sxs-lookup"><span data-stu-id="4371f-166">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4371f-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4371f-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4371f-168">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4371f-168">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    <span data-ttu-id="4371f-169">Aby uzyskać więcej informacji na temat konfigurowania tożsamości, zobacz [konfigurowania tożsamości](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="4371f-169">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="4371f-170">Można także skonfigurować typ danych klucza podstawowego, zobacz [typu danych kluczy podstawowych konfigurowania tożsamości](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="4371f-170">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="4371f-171">Wyświetl bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4371f-171">View the database.</span></span>

    <span data-ttu-id="4371f-172">Jeśli aplikacja używa bazy danych programu SQL Server (domyślnie w systemie Windows, jak i dla użytkowników programu Visual Studio), można wyświetlić baza danych utworzona aplikacja.</span><span class="sxs-lookup"><span data-stu-id="4371f-172">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="4371f-173">Można użyć **programu SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="4371f-173">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="4371f-174">W programie Visual Studio, wybierz opcję **widoku** > **Eksplorator obiektów SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="4371f-174">Alternatively, from Visual Studio, select **View** > **SQL Server Object Explorer**.</span></span> <span data-ttu-id="4371f-175">Połączyć się z **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="4371f-175">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="4371f-176">Baza danych o nazwie odpowiadającej **aspnet — <*Nazwa projektu*>-<*Data ciąg* >**  jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="4371f-176">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Menu kontekstowe w tabeli bazy danych AspNetUsers](identity/_static/04-db.png)
    
    <span data-ttu-id="4371f-178">Rozwiń bazę danych i jego **tabel**, kliknij prawym przyciskiem myszy **dbo. AspNetUsers** tabeli i wybierz **danych widoku**.</span><span class="sxs-lookup"><span data-stu-id="4371f-178">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

8. <span data-ttu-id="4371f-179">Weryfikowanie działania tożsamości</span><span class="sxs-lookup"><span data-stu-id="4371f-179">Verify Identity works</span></span>

    <span data-ttu-id="4371f-180">Wartość domyślna *aplikacji sieci Web platformy ASP.NET Core* szablon projektu umożliwia użytkownikom uzyskiwanie dostępu do żadnych czynności w aplikacji bez potrzeby logowania.</span><span class="sxs-lookup"><span data-stu-id="4371f-180">The default *ASP.NET Core Web Application* project template allows users to access any action in the application without having to login.</span></span> <span data-ttu-id="4371f-181">Aby sprawdzić, czy działa tożsamości platformy ASP.NET, należy dodać`[Authorize]` atrybutu `About` akcji `Home` kontrolera.</span><span class="sxs-lookup"><span data-stu-id="4371f-181">To verify that ASP.NET Identity works, add an`[Authorize]` attribute to the `About` action of the `Home` Controller.</span></span>
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4371f-182">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4371f-182">Visual Studio</span></span>](#tab/visual-studio)

    <span data-ttu-id="4371f-183">Uruchom projekt za pomocą **Ctrl** + **F5** i przejdź do **o** strony.</span><span class="sxs-lookup"><span data-stu-id="4371f-183">Run the project using **Ctrl** + **F5** and navigate to the **About** page.</span></span> <span data-ttu-id="4371f-184">Tylko uwierzytelnieni użytkownicy mogą uzyskać dostępu do **o** strony, dlatego ASP.NET przekieruje Cię do strony logowania, aby zalogować się lub zarejestrować.</span><span class="sxs-lookup"><span data-stu-id="4371f-184">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4371f-185">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="4371f-185">.NET Core CLI</span></span>](#tab/netcore-cli)

    <span data-ttu-id="4371f-186">Otwórz okno polecenia i przejdź do katalogu głównego projektu zawierającego katalogu `.csproj` pliku.</span><span class="sxs-lookup"><span data-stu-id="4371f-186">Open a command window and navigate to the project's root directory containing the `.csproj` file.</span></span> <span data-ttu-id="4371f-187">Uruchom `dotnet run` polecenie do uruchomienia aplikacji:</span><span class="sxs-lookup"><span data-stu-id="4371f-187">Run the `dotnet run` command to run the app:</span></span>

    ```cs
    dotnet run 
    ```

    <span data-ttu-id="4371f-188">Przeglądaj adres URL określony w danych wyjściowych z `dotnet run` polecenia.</span><span class="sxs-lookup"><span data-stu-id="4371f-188">Browse the URL specified in the output from the `dotnet run` command.</span></span> <span data-ttu-id="4371f-189">Ten adres URL powinien wskazywać `localhost` z numeru portu wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="4371f-189">The URL should point to `localhost` with a generated port number.</span></span> <span data-ttu-id="4371f-190">Przejdź do **o** strony.</span><span class="sxs-lookup"><span data-stu-id="4371f-190">Navigate to the **About** page.</span></span> <span data-ttu-id="4371f-191">Tylko uwierzytelnieni użytkownicy mogą uzyskać dostępu do **o** strony, dlatego ASP.NET przekieruje Cię do strony logowania, aby zalogować się lub zarejestrować.</span><span class="sxs-lookup"><span data-stu-id="4371f-191">Only authenticated users may access the **About** page now, so ASP.NET redirects you to the login page to login or register.</span></span>

    ---

## <a name="identity-components"></a><span data-ttu-id="4371f-192">Składniki tożsamości</span><span class="sxs-lookup"><span data-stu-id="4371f-192">Identity Components</span></span>

<span data-ttu-id="4371f-193">Zestaw odwołania podstawowego dla systemu tożsamości jest `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="4371f-193">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="4371f-194">Ten pakiet zawiera podstawowy zestaw interfejsów dla platformy ASP.NET Core tożsamości i jest dołączony przez `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="4371f-194">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="4371f-195">Te zależności są niezbędne do używania systemu tożsamości w aplikacji platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="4371f-195">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="4371f-196">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Zawiera typy wymaganych do korzystania z tożsamości z programu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="4371f-196">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="4371f-197">`Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core jest technologii dostępu do danych zalecane przez firmę Microsoft relacyjnych baz danych, takich jak SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4371f-197">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="4371f-198">Do testowania, można użyć `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="4371f-198">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="4371f-199">`Microsoft.AspNetCore.Authentication.Cookies`-Oprogramowanie pośredniczące, które umożliwia aplikacji korzystanie z uwierzytelniania opartego na pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="4371f-199">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="4371f-200">Migrowanie tożsamości platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4371f-200">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="4371f-201">Aby uzyskać dodatkowe informacje i wskazówki dotyczące migrowania istniejących tożsamości przechowywania można znaleźć [Migrowanie uwierzytelnianie i tożsamość](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="4371f-201">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4371f-202">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="4371f-202">Next Steps</span></span>

* [<span data-ttu-id="4371f-203">Migrowanie uwierzytelnianie i tożsamość</span><span class="sxs-lookup"><span data-stu-id="4371f-203">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="4371f-204">Potwierdzenie konta i odzyskiwanie hasła</span><span class="sxs-lookup"><span data-stu-id="4371f-204">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="4371f-205">Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS</span><span class="sxs-lookup"><span data-stu-id="4371f-205">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="4371f-206">Włączanie uwierzytelniania za pomocą usługi Facebook, Google i innych dostawców zewnętrznych</span><span class="sxs-lookup"><span data-stu-id="4371f-206">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)