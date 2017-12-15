---
title: "Wprowadzenie do tożsamości na platformy ASP.NET Core"
author: rick-anderson
description: "Użyj tożsamości w aplikacji platformy ASP.NET Core"
keywords: "Platformy ASP.NET Core tożsamości, autoryzacji, zabezpieczeń"
ms.author: riande
manager: wpickett
ms.date: 12/15/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-547ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity
ms.openlocfilehash: 7daf0267a6dc659afbd188ce87e35ca40816a31d
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Wprowadzenie do tożsamości na platformy ASP.NET Core

Przez [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Dykstra Tomasz](https://github.com/tdykstra), Galloway Jan [Erik Reitan](https://github.com/Erikre), i [Steve Smith](https://ardalis.com/)

Tożsamość platformy ASP.NET Core to system członkostwa, co pozwala na dodawanie funkcji logowania do aplikacji. Użytkownicy mogą tworzyć konta i logowania przy użyciu nazwy użytkownika i hasło lub użyć dostawcy logowania zewnętrznego, takich jak Facebook, Google, Microsoft Account, Twitter lub innych użytkowników.

Można skonfigurować ASP.NET Identity Core używać bazy danych programu SQL Server do przechowywania nazwy użytkownika, hasła i danych profilu. Alternatywnie można użyć własnych magazynu trwałego, na przykład magazynu tabel Azure. Ten dokument zawiera instrukcje dla programu Visual Studio i przy użyciu interfejsu wiersza polecenia.

## <a name="overview-of-identity"></a>Omówienie tożsamości

W tym temacie będzie używanie ASP.NET Core Identity funkcje, aby zarejestrować, zaloguj się i wylogowania użytkownika. Bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji korzystających z platformy ASP.NET Identity Core zobacz sekcję następne kroki na końcu tego artykułu.

1.  Tworzenie projektu aplikacji sieci Web platformy ASP.NET Core z indywidualnych kont użytkowników.

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)
    W programie Visual Studio, wybierz **pliku** -> **nowy** -> **projektu**. Wybierz **aplikacji sieci Web ASP.NET** z **nowy projekt** okno dialogowe. Wybieranie platformy ASP.NET Core **Web Application(Model-View-Controller)** dla platformy ASP.NET Core 2.x z **indywidualnych kont użytkowników** jako metody uwierzytelniania.

    Uwaga: Należy wybrać **indywidualnych kont użytkowników**.
 
    ![Okno dialogowe nowego projektu](identity/_static/01-mvc_2.png)
    
    # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)
    Jeśli używasz interfejsu wiersza polecenia platformy .NET Core, Utwórz nowy projekt za pomocą ``dotnet new mvc --auth Individual``. To polecenie tworzy nowy projekt z tego samego kodu szablonu tożsamości tworzonych w Visual Studio.
 
    Utworzony projekt zawiera `Microsoft.AspNetCore.Identity.EntityFrameworkCore` pakiet, który będzie się powtarzać, dane tożsamości i schematu przy użyciu programu SQL Server [Entity Framework Core](https://docs.microsoft.com/ef/).
    
    ---
 
2.  Konfigurowanie usługi tożsamości i Dodaj oprogramowanie pośredniczące w `Startup`.

    Usługi tożsamości są dodawane do aplikacji w `ConfigureServices` metoda `Startup` klasy:

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    Te usługi są udostępniane dla aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).
    
    Tożsamość jest włączone dla aplikacji przez wywołanie metody `UseAuthentication` w `Configure` metody. `UseAuthentication`dodaje uwierzytelniania [oprogramowanie pośredniczące](xref:fundamentals/middleware) do potoku żądania.
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-34)]
    
    Te usługi są udostępniane dla aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).
    
    Tożsamość jest włączone dla aplikacji przez wywołanie metody `UseIdentity` w `Configure` metody. `UseIdentity`dodaje plik cookie uwierzytelniania [oprogramowanie pośredniczące](xref:fundamentals/middleware) do potoku żądania.
        
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    Aby uzyskać więcej informacji na temat uruchamiania aplikacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup).

3.  Utwórz użytkownika.

    Uruchom aplikację, a następnie kliknij polecenie **zarejestrować** łącza.

    Jeśli wykonujesz tę akcję po raz pierwszy, może być wymagany do uruchamiania migracji. Aplikacja wyświetli monit o **zastosować migracje**:
    
    ![Zastosuj stronę sieci Web migracji](identity/_static/apply-migrations.png)
    
    Alternatywnie można testować przy użyciu ASP.NET Core Identity z aplikacji bez trwałego bazy danych przy użyciu bazy danych w pamięci. Aby użyć bazy danych w pamięci, należy dodać ``Microsoft.EntityFrameworkCore.InMemory`` pakiet do aplikacji i zmodyfikuj wywołanie aplikacji ``AddDbContext`` w ``ConfigureServices`` w następujący sposób:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    Po kliknięciu przez użytkownika **zarejestrować** łącza, ``Register`` akcji jest wywoływana na ``AccountController``. ``Register`` Akcja tworzy użytkownika, wywołując `CreateAsync` na `_userManager` obiektu (podano ``AccountController`` przez iniekcji zależności):
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    Jeśli użytkownik został pomyślnie utworzony, użytkownik jest zalogowany przez wywołanie ``_signInManager.SignInAsync``.

    **Uwaga:** zobacz [konta potwierdzenia](xref:security/authentication/accconfirm#prevent-login-at-registration) kroki zapobiec bezpośredniego logowania podczas rejestracji.
 
4.  Zaloguj się.
 
    Użytkownicy mogą rejestrować klikając **Zaloguj** łącze u góry strony, lub mogą zostać przesłane do strony logowania, gdy próbują uzyskać dostępu do części witryny, która wymaga autoryzacji. Gdy użytkownik przesyła formularz na stronie logowania ``AccountController`` ``Login`` nosi nazwę akcji.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    ``Login`` Wywołania akcji ``PasswordSignInAsync`` na ``_signInManager`` obiektu (podano ``AccountController`` przez iniekcji zależności).
 
    Podstawowym ``Controller`` klasy ujawnia ``User`` właściwości, którego można korzystać z metod kontrolera. Na przykład można wyliczyć `User.Claims` i podejmowania decyzji dotyczących autoryzacji. Aby uzyskać więcej informacji, zobacz [autoryzacji](xref:security/authorization/index).
 
5.  Wyloguj się.
 
    Kliknięcie przycisku **Wyloguj się** link wywołania `LogOut` akcji.
 
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    Poprzedni kod powyżej wywołania `_signInManager.SignOutAsync` metody. `SignOutAsync` Metody czyści oświadczeń użytkownika przechowywane w pliku cookie.
 
6.  Konfiguracja.

    Tożsamość ma pewne domyślne zachowania przesłaniające w klasie uruchomienia aplikacji. Nie ma potrzeby konfigurowania ``IdentityOptions`` Jeśli korzystasz z domyślnego zachowania.

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[Program ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-39)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[Program ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-34)]

    ---
    
    Aby uzyskać więcej informacji na temat konfigurowania tożsamości, zobacz [konfigurowania tożsamości](xref:security/authentication/identity-configuration).
    
    Można także skonfigurować typ danych klucza podstawowego, zobacz [typu danych kluczy podstawowych konfigurowania tożsamości](xref:security/authentication/identity-primary-key-configuration).
 
7.  Wyświetl bazy danych.

    Jeśli aplikacja używa bazy danych programu SQL Server (domyślnie w systemie Windows, jak i dla użytkowników programu Visual Studio), można wyświetlić baza danych utworzona aplikacja. Można użyć **programu SQL Server Management Studio**. W programie Visual Studio, wybierz opcję **widoku** -> **Eksplorator obiektów SQL Server**. Połączyć się z **(localdb) \MSSQLLocalDB**. Baza danych o nazwie odpowiadającej  **aspnet — <*Nazwa projektu*>-<*Data ciąg*> ** jest wyświetlany.

    ![Menu kontekstowe w tabeli bazy danych AspNetUsers](identity/_static/04-db.png)
    
    Rozwiń bazę danych i jego **tabel**, kliknij prawym przyciskiem myszy **dbo. AspNetUsers** tabeli i wybierz **danych widoku**.

## <a name="identity-components"></a>Składniki tożsamości

Zestaw odwołania podstawowego dla systemu tożsamości jest `Microsoft.AspNetCore.Identity`. Ten pakiet zawiera podstawowy zestaw interfejsów dla platformy ASP.NET Core tożsamości i jest dołączony przez `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Te zależności są niezbędne do używania systemu tożsamości w aplikacji platformy ASP.NET Core:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore`-Zawiera typy wymaganych do korzystania z tożsamości z programu Entity Framework Core.

* `Microsoft.EntityFrameworkCore.SqlServer`-Entity Framework Core jest technologii dostępu do danych zalecane przez firmę Microsoft relacyjnych baz danych, takich jak SQL Server. Do testowania, można użyć `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies`-Oprogramowanie pośredniczące, które umożliwia aplikacji korzystanie z uwierzytelniania opartego na pliku cookie.

## <a name="migrating-to-aspnet-core-identity"></a>Migrowanie tożsamości platformy ASP.NET Core

Aby uzyskać dodatkowe informacje i wskazówki dotyczące migrowania istniejących tożsamości przechowywania można znaleźć [Migrowanie uwierzytelnianie i tożsamość](xref:migration/identity).

## <a name="next-steps"></a>Następne kroki

* [Migrowanie uwierzytelnianie i tożsamość](xref:migration/identity)
* [Potwierdzenie konta i hasła odzyskiwania](xref:security/authentication/accconfirm)
* [Uwierzytelnianie dwuskładnikowe z programem SMS](xref:security/authentication/2fa)
* [Włączanie uwierzytelniania za pomocą usługi Facebook, Google i innych dostawców zewnętrznych](xref:security/authentication/social/index)
