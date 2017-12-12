---
title: "Wprowadzenie do tożsamości na platformy ASP.NET Core"
author: rick-anderson
description: "Przy użyciu tożsamości z aplikacji platformy ASP.NET Core"
keywords: "Platformy ASP.NET Core tożsamości, autoryzacji, zabezpieczeń"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 0679663b3b3b66f9935d0fb24360be2954fcdee1
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a><span data-ttu-id="f2158-104">Wprowadzenie do tożsamości na platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f2158-104">Introduction to Identity on ASP.NET Core</span></span>

<span data-ttu-id="f2158-105">Przez [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Dykstra Tomasz](https://github.com/tdykstra), Galloway Jan [Erik Reitan](https://github.com/Erikre), i [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f2158-105">By [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Jon Galloway, [Erik Reitan](https://github.com/Erikre), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f2158-106">Tożsamość platformy ASP.NET Core to system członkostwa, co pozwala na dodawanie funkcji logowania do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f2158-106">ASP.NET Core Identity is a membership system which allows you to add login functionality to your application.</span></span> <span data-ttu-id="f2158-107">Użytkownicy mogą tworzyć konta i logowania przy użyciu nazwy użytkownika i hasło lub użyć dostawcy logowania zewnętrznego, takich jak Facebook, Google, Microsoft Account, Twitter lub innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="f2158-107">Users can create an account and login with a user name and password or they can use an external login provider such as Facebook, Google, Microsoft Account, Twitter or others.</span></span>

<span data-ttu-id="f2158-108">Można skonfigurować ASP.NET Identity Core używać bazy danych programu SQL Server do przechowywania nazwy użytkownika, hasła i danych profilu.</span><span class="sxs-lookup"><span data-stu-id="f2158-108">You can configure ASP.NET Core Identity to use a SQL Server database to store user names, passwords, and profile data.</span></span> <span data-ttu-id="f2158-109">Alternatywnie można użyć własnych magazynu trwałego, na przykład magazynu tabel platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="f2158-109">Alternatively, you can use your own persistent store, for example Azure Table Storage.</span></span> <span data-ttu-id="f2158-110">Ten dokument zawiera instrukcje dla programu Visual Studio i przy użyciu interfejsu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="f2158-110">This document contains instructions for Visual Studio and for using the CLI.</span></span>

## <a name="overview-of-identity"></a><span data-ttu-id="f2158-111">Omówienie tożsamości</span><span class="sxs-lookup"><span data-stu-id="f2158-111">Overview of Identity</span></span>

<span data-ttu-id="f2158-112">W tym temacie będzie używanie ASP.NET Core Identity funkcje, aby zarejestrować, zaloguj się i wylogowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f2158-112">In this topic, you'll learn how to use ASP.NET Core Identity to add functionality to register, log in, and log out a user.</span></span> <span data-ttu-id="f2158-113">Bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji korzystających z platformy ASP.NET Identity Core zobacz sekcję następne kroki na końcu tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="f2158-113">For more detailed instructions about creating apps using ASP.NET Core Identity, see the Next Steps section at the end of this article.</span></span>

1.  <span data-ttu-id="f2158-114">Tworzenie projektu aplikacji sieci Web platformy ASP.NET Core z indywidualnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="f2158-114">Create an ASP.NET Core Web Application project with Individual User Accounts.</span></span>

    # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f2158-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f2158-115">Visual Studio</span></span>](#tab/visual-studio)
    <span data-ttu-id="f2158-116">W programie Visual Studio, wybierz **pliku** -> **nowy** -> **projektu**.</span><span class="sxs-lookup"><span data-stu-id="f2158-116">In Visual Studio, select **File** -> **New** -> **Project**.</span></span> <span data-ttu-id="f2158-117">Wybierz **aplikacji sieci Web ASP.NET** z **nowy projekt** okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="f2158-117">Select the **ASP.NET Web Application** from the **New Project** dialog box.</span></span> <span data-ttu-id="f2158-118">Wybieranie platformy ASP.NET Core **Web Application(Model-View-Controller)** dla platformy ASP.NET Core 2.x z **indywidualnych kont użytkowników** jako metody uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="f2158-118">Selecting an ASP.NET Core **Web Application(Model-View-Controller)** for ASP.NET Core 2.x with **Individual User Accounts** as the authentication method.</span></span>

    <span data-ttu-id="f2158-119">Uwaga: Należy wybrać **indywidualnych kont użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="f2158-119">Note: You must select **Individual User Accounts**.</span></span>
 
    ![Okno dialogowe nowego projektu](identity/_static/01-mvc_2.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f2158-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="f2158-121">.NET Core CLI</span></span>](#tab/netcore-cli)
    <span data-ttu-id="f2158-122">Jeśli używasz interfejsu wiersza polecenia platformy .NET Core, Utwórz nowy projekt za pomocą ``dotnet new mvc --auth Individual``.</span><span class="sxs-lookup"><span data-stu-id="f2158-122">If using the .NET Core CLI, create the new project using ``dotnet new mvc --auth Individual``.</span></span> <span data-ttu-id="f2158-123">Spowoduje to utworzenie nowego projektu z tego samego kodu szablonu tożsamości tworzonych w Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f2158-123">This will create a new project with the same Identity template code Visual Studio creates.</span></span>
 
    <span data-ttu-id="f2158-124">Utworzony projekt zawiera `Microsoft.AspNetCore.Identity.EntityFrameworkCore` pakiet, który będzie umieszczony dane tożsamości i schematu przy użyciu programu SQL Server [Entity Framework Core](https://docs.microsoft.com/ef/).</span><span class="sxs-lookup"><span data-stu-id="f2158-124">The created project contains the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` package, which will persist the Identity data and schema to SQL Server using [Entity Framework Core](https://docs.microsoft.com/ef/).</span></span>
    
    ---
 
2.  <span data-ttu-id="f2158-125">Konfigurowanie usługi tożsamości i Dodaj oprogramowanie pośredniczące w `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f2158-125">Configure Identity services and add middleware in `Startup`.</span></span>

    <span data-ttu-id="f2158-126">Usługi tożsamości są dodawane do aplikacji w `ConfigureServices` metoda `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="f2158-126">The Identity services are added to the application in the `ConfigureServices` method in the `Startup` class:</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f2158-127">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f2158-127">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    <span data-ttu-id="f2158-128">Te usługi są udostępniane dla aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f2158-128">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="f2158-129">Tożsamość jest włączone dla aplikacji przez wywołanie metody `UseAuthentication` w `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="f2158-129">Identity is enabled for the application by calling `UseAuthentication` in the `Configure` method.</span></span> <span data-ttu-id="f2158-130">`UseAuthentication`dodaje uwierzytelniania [oprogramowanie pośredniczące](xref:fundamentals/middleware) do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="f2158-130">`UseAuthentication` adds authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f2158-131">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f2158-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    <span data-ttu-id="f2158-132">Te usługi są udostępniane dla aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f2158-132">These services are made available to the application through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
    
    <span data-ttu-id="f2158-133">Tożsamość jest włączone dla aplikacji przez wywołanie metody `UseIdentity` w `Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="f2158-133">Identity is enabled for the application by calling `UseIdentity` in the `Configure` method.</span></span> <span data-ttu-id="f2158-134">`UseIdentity`dodaje plik cookie uwierzytelniania [oprogramowanie pośredniczące](xref:fundamentals/middleware) do potoku żądania.</span><span class="sxs-lookup"><span data-stu-id="f2158-134">`UseIdentity` adds cookie-based authentication [middleware](xref:fundamentals/middleware) to the request pipeline.</span></span>
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    <span data-ttu-id="f2158-135">Aby uzyskać więcej informacji na temat uruchamiania aplikacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="f2158-135">For more information about the application start up process, see [Application Startup](xref:fundamentals/startup).</span></span>

3.  <span data-ttu-id="f2158-136">Utwórz użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f2158-136">Create a user.</span></span>

    <span data-ttu-id="f2158-137">Uruchom aplikację, a następnie kliknij polecenie **zarejestrować** łącza.</span><span class="sxs-lookup"><span data-stu-id="f2158-137">Launch the application and then click on the **Register** link.</span></span>

    <span data-ttu-id="f2158-138">Jeśli wykonujesz tę akcję po raz pierwszy, może być wymagany do uruchamiania migracji.</span><span class="sxs-lookup"><span data-stu-id="f2158-138">If this is the first time you're performing this action, you may be required to run migrations.</span></span> <span data-ttu-id="f2158-139">Aplikacja wyświetli monit o **zastosować migracje**:</span><span class="sxs-lookup"><span data-stu-id="f2158-139">The application prompts you to **Apply Migrations**:</span></span>
    
    ![Zastosuj stronę sieci Web migracji](identity/_static/apply-migrations.png)
    
    <span data-ttu-id="f2158-141">Alternatywnie można testować przy użyciu ASP.NET Core Identity z aplikacji bez trwałego bazy danych przy użyciu bazy danych w pamięci.</span><span class="sxs-lookup"><span data-stu-id="f2158-141">Alternately, you can test using ASP.NET Core Identity with your app without a persistent database by using an in-memory database.</span></span> <span data-ttu-id="f2158-142">Aby użyć bazy danych w pamięci, należy dodać ``Microsoft.EntityFrameworkCore.InMemory`` pakiet do aplikacji i zmodyfikuj wywołanie aplikacji ``AddDbContext`` w ``ConfigureServices`` w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f2158-142">To use an in-memory database, add the ``Microsoft.EntityFrameworkCore.InMemory`` package to your app and modify your app's call to ``AddDbContext`` in ``ConfigureServices`` as follows:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    <span data-ttu-id="f2158-143">Po kliknięciu przez użytkownika **zarejestrować** łącza, ``Register`` akcji jest wywoływana na ``AccountController``.</span><span class="sxs-lookup"><span data-stu-id="f2158-143">When the user clicks the **Register** link, the ``Register`` action is invoked on ``AccountController``.</span></span> <span data-ttu-id="f2158-144">``Register`` Akcja tworzy użytkownika, wywołując `CreateAsync` na `_userManager` obiektu (podano ``AccountController`` przez iniekcji zależności):</span><span class="sxs-lookup"><span data-stu-id="f2158-144">The ``Register`` action creates the user by calling `CreateAsync` on the  `_userManager` object (provided to ``AccountController`` by dependency injection):</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    <span data-ttu-id="f2158-145">Jeśli użytkownik został pomyślnie utworzony, użytkownik jest zalogowany przez wywołanie ``_signInManager.SignInAsync``.</span><span class="sxs-lookup"><span data-stu-id="f2158-145">If the user was created successfully, the user is logged in by the call to ``_signInManager.SignInAsync``.</span></span>

    <span data-ttu-id="f2158-146">**Uwaga:** zobacz [konta potwierdzenia](xref:security/authentication/accconfirm#prevent-login-at-registration) kroki zapobiec bezpośredniego logowania podczas rejestracji.</span><span class="sxs-lookup"><span data-stu-id="f2158-146">**Note:** See [account confirmation](xref:security/authentication/accconfirm#prevent-login-at-registration) for steps to prevent immediate login at registration.</span></span>
 
4.  <span data-ttu-id="f2158-147">Zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="f2158-147">Log in.</span></span>
 
    <span data-ttu-id="f2158-148">Użytkownicy mogą rejestrować klikając **Zaloguj** łącze u góry strony, lub mogą zostać przesłane do strony logowania, gdy próbują uzyskać dostępu do części witryny, która wymaga autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="f2158-148">Users can sign in by clicking the **Log in** link at the top of the site, or they may be navigated to the Login page if they attempt to access a part of the site that requires authorization.</span></span> <span data-ttu-id="f2158-149">Gdy użytkownik przesyła formularz na stronie logowania ``AccountController`` ``Login`` nosi nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="f2158-149">When the user submits the form on the Login page, the ``AccountController`` ``Login`` action is called.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    <span data-ttu-id="f2158-150">``Login`` Wywołania akcji ``PasswordSignInAsync`` na ``_signInManager`` obiektu (podano ``AccountController`` przez iniekcji zależności).</span><span class="sxs-lookup"><span data-stu-id="f2158-150">The ``Login`` action calls ``PasswordSignInAsync`` on the ``_signInManager`` object (provided to ``AccountController`` by dependency injection).</span></span>
 
    <span data-ttu-id="f2158-151">Podstawowym ``Controller`` klasy ujawnia ``User`` właściwości, którego można korzystać z metod kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f2158-151">The base ``Controller`` class exposes a ``User`` property that you can access from controller methods.</span></span> <span data-ttu-id="f2158-152">Na przykład można wyliczyć `User.Claims` i podejmowania decyzji dotyczących autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="f2158-152">For instance, you can enumerate `User.Claims` and make authorization decisions.</span></span> <span data-ttu-id="f2158-153">Aby uzyskać więcej informacji, zobacz [autoryzacji](xref:security/authorization/index).</span><span class="sxs-lookup"><span data-stu-id="f2158-153">For more information, see [Authorization](xref:security/authorization/index).</span></span>
 
5.  <span data-ttu-id="f2158-154">Wyloguj się.</span><span class="sxs-lookup"><span data-stu-id="f2158-154">Log out.</span></span>
 
    <span data-ttu-id="f2158-155">Kliknięcie przycisku **Wyloguj się** link wywołania `LogOut` akcji.</span><span class="sxs-lookup"><span data-stu-id="f2158-155">Clicking the **Log out** link calls the `LogOut` action.</span></span>
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    <span data-ttu-id="f2158-156">Poprzedni kod powyżej wywołania `_signInManager.SignOutAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="f2158-156">The preceding code above calls the `_signInManager.SignOutAsync` method.</span></span> <span data-ttu-id="f2158-157">`SignOutAsync` Metody czyści oświadczeń użytkownika przechowywane w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="f2158-157">The `SignOutAsync` method clears the user's claims stored in a cookie.</span></span>
 
6.  <span data-ttu-id="f2158-158">Konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="f2158-158">Configuration.</span></span>

    <span data-ttu-id="f2158-159">Tożsamość ma pewne domyślne zachowania przesłaniające w klasie uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f2158-159">Identity has some default behaviors that you can override in your application's startup class.</span></span> <span data-ttu-id="f2158-160">Nie ma potrzeby konfigurowania ``IdentityOptions`` Jeśli korzystasz z domyślnego zachowania.</span><span class="sxs-lookup"><span data-stu-id="f2158-160">You do not need to configure ``IdentityOptions`` if you are using the default behaviors.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f2158-161">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f2158-161">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f2158-162">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f2158-162">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    <span data-ttu-id="f2158-163">Aby uzyskać więcej informacji na temat konfigurowania tożsamości, zobacz [konfigurowania tożsamości](xref:security/authentication/identity-configuration).</span><span class="sxs-lookup"><span data-stu-id="f2158-163">For more information about how to configure Identity, see [Configure Identity](xref:security/authentication/identity-configuration).</span></span>
    
    <span data-ttu-id="f2158-164">Można także skonfigurować typ danych klucza podstawowego, zobacz [typu danych kluczy podstawowych konfigurowania tożsamości](xref:security/authentication/identity-primary-key-configuration).</span><span class="sxs-lookup"><span data-stu-id="f2158-164">You also can configure the data type of the primary key, see [Configure Identity primary keys data type](xref:security/authentication/identity-primary-key-configuration).</span></span>
 
7.  <span data-ttu-id="f2158-165">Wyświetl bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f2158-165">View the database.</span></span>

    <span data-ttu-id="f2158-166">Jeśli aplikacja używa bazy danych programu SQL Server (domyślnie w systemie Windows, jak i dla użytkowników programu Visual Studio), można wyświetlić baza danych utworzona aplikacja.</span><span class="sxs-lookup"><span data-stu-id="f2158-166">If your app is using a SQL Server database (the default on Windows and for Visual Studio users), you can view the database the app created.</span></span> <span data-ttu-id="f2158-167">Można użyć **programu SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="f2158-167">You can use **SQL Server Management Studio**.</span></span> <span data-ttu-id="f2158-168">W programie Visual Studio, wybierz opcję **widoku** -> **Eksplorator obiektów SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="f2158-168">Alternatively, from Visual Studio, select **View** -> **SQL Server Object Explorer**.</span></span> <span data-ttu-id="f2158-169">Połączyć się z **(localdb) \MSSQLLocalDB**.</span><span class="sxs-lookup"><span data-stu-id="f2158-169">Connect to **(localdb)\MSSQLLocalDB**.</span></span> <span data-ttu-id="f2158-170">Baza danych o nazwie odpowiadającej  **aspnet — <*Nazwa projektu*>-<*Data ciąg*> ** jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="f2158-170">The database with a name matching **aspnet-<*name of your project*>-<*date string*>** is displayed.</span></span>

    ![Menu kontekstowe w tabeli bazy danych AspNetUsers](identity/_static/04-db.png)
    
    <span data-ttu-id="f2158-172">Rozwiń bazę danych i jego **tabel**, kliknij prawym przyciskiem myszy **dbo. AspNetUsers** tabeli i wybierz **danych widoku**.</span><span class="sxs-lookup"><span data-stu-id="f2158-172">Expand the database and its **Tables**, then right-click the **dbo.AspNetUsers** table and select **View Data**.</span></span>

## <a name="identity-components"></a><span data-ttu-id="f2158-173">Składniki tożsamości</span><span class="sxs-lookup"><span data-stu-id="f2158-173">Identity Components</span></span>

<span data-ttu-id="f2158-174">Zestaw odwołania podstawowego dla systemu tożsamości jest `Microsoft.AspNetCore.Identity`.</span><span class="sxs-lookup"><span data-stu-id="f2158-174">The primary reference assembly for the Identity system is `Microsoft.AspNetCore.Identity`.</span></span> <span data-ttu-id="f2158-175">Ten pakiet zawiera podstawowy zestaw interfejsów dla platformy ASP.NET Core tożsamości i jest dołączony przez `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="f2158-175">This package contains the core set of interfaces for ASP.NET Core Identity, and is included by `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.</span></span>

<span data-ttu-id="f2158-176">Te zależności są niezbędne do używania systemu tożsamości w aplikacji platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="f2158-176">These dependencies are needed to use the Identity system in ASP.NET Core applications:</span></span>

* <span data-ttu-id="f2158-177">`Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Zawiera typy wymaganych do korzystania z tożsamości z programu Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="f2158-177">`Microsoft.AspNetCore.Identity.EntityFrameworkCore` - Contains the required types to use Identity with Entity Framework Core.</span></span>

* <span data-ttu-id="f2158-178">`Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core jest technologii dostępu do danych zalecane przez firmę Microsoft relacyjnych baz danych, takich jak SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f2158-178">`Microsoft.EntityFrameworkCore.SqlServer` - Entity Framework Core is Microsoft's recommended data access technology for relational databases like SQL Server.</span></span> <span data-ttu-id="f2158-179">Do testowania, można użyć `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="f2158-179">For testing, you can use `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

* <span data-ttu-id="f2158-180">`Microsoft.AspNetCore.Authentication.Cookies`-Oprogramowanie pośredniczące, które umożliwia aplikacji korzystanie z uwierzytelniania opartego na pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="f2158-180">`Microsoft.AspNetCore.Authentication.Cookies` - Middleware that enables an app to use cookie-based authentication.</span></span>

## <a name="migrating-to-aspnet-core-identity"></a><span data-ttu-id="f2158-181">Migrowanie tożsamości platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f2158-181">Migrating to ASP.NET Core Identity</span></span>

<span data-ttu-id="f2158-182">Aby uzyskać dodatkowe informacje i wskazówki dotyczące migrowania istniejących tożsamości przechowywania można znaleźć [Migrowanie uwierzytelnianie i tożsamość](xref:migration/identity).</span><span class="sxs-lookup"><span data-stu-id="f2158-182">For additional information and guidance on migrating your existing Identity store see [Migrating Authentication and Identity](xref:migration/identity).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f2158-183">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="f2158-183">Next Steps</span></span>

* [<span data-ttu-id="f2158-184">Migrowanie uwierzytelnianie i tożsamość</span><span class="sxs-lookup"><span data-stu-id="f2158-184">Migrating Authentication and Identity</span></span>](xref:migration/identity)
* [<span data-ttu-id="f2158-185">Potwierdzenie konta i hasła odzyskiwania</span><span class="sxs-lookup"><span data-stu-id="f2158-185">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="f2158-186">Uwierzytelnianie dwuskładnikowe z programem SMS</span><span class="sxs-lookup"><span data-stu-id="f2158-186">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="f2158-187">Włączanie uwierzytelniania za pomocą usługi Facebook, Google i innych dostawców zewnętrznych</span><span class="sxs-lookup"><span data-stu-id="f2158-187">Enabling authentication using Facebook, Google and other external providers</span></span>](xref:security/authentication/social/index)
