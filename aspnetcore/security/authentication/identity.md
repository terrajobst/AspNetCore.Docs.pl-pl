---
title: "Wprowadzenie do tożsamości na platformy ASP.NET Core"
author: rick-anderson
description: "Tożsamość aplikacji korzystać z platformy ASP.NET Core. Zawiera wymagania dotyczące hasła ustawienie (RequireDigit, RequiredLength, RequiredUniqueChars i inne)."
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity
ms.openlocfilehash: a84c5f1d4cf802ee0c4116d2a02bdbfbab9aa72b
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Wprowadzenie do tożsamości na platformy ASP.NET Core

Przez [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Dykstra Tomasz](https://github.com/tdykstra), Galloway Jan [Erik Reitan](https://github.com/Erikre), i [Steve Smith](https://ardalis.com/)

Tożsamość platformy ASP.NET Core to system członkostwa, co pozwala na dodawanie funkcji logowania do aplikacji. Użytkownicy mogą tworzyć konta i logowania przy użyciu nazwy użytkownika i hasło lub użyć dostawcy logowania zewnętrznego, takich jak Facebook, Google, Microsoft Account, Twitter lub innych użytkowników.

Można skonfigurować ASP.NET Identity Core używać bazy danych programu SQL Server do przechowywania nazwy użytkownika, hasła i danych profilu. Alternatywnie można użyć własnych magazynu trwałego, na przykład magazynu tabel Azure. Ten dokument zawiera instrukcje dla programu Visual Studio i przy użyciu interfejsu wiersza polecenia.

[Wyświetl lub pobrać przykładowy kod.](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(Jak pobrać)](https://docs.microsoft.com/aspnet/core/tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>Omówienie tożsamości

W tym temacie będzie używanie ASP.NET Core Identity funkcje, aby zarejestrować, zaloguj się i wylogowania użytkownika. Bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji korzystających z platformy ASP.NET Identity Core zobacz sekcję następne kroki na końcu tego artykułu.

1.  Tworzenie projektu aplikacji sieci Web platformy ASP.NET Core z indywidualnych kont użytkowników.

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    W programie Visual Studio, wybierz **pliku** > **nowy** > **projektu**. Wybierz **aplikacji sieci Web platformy ASP.NET Core** i kliknij przycisk **OK**.

    ![Okno dialogowe nowego projektu](identity/_static/01-new-project.png)

    Wybierz platformy ASP.NET Core **aplikacji sieci Web (Model-View-Controller)** dla platformy ASP.NET Core 2.x, a następnie wybierz **Zmień uwierzytelnianie**.

    ![Okno dialogowe nowego projektu](identity/_static/02-new-project.png)

    Zostanie wyświetlone okno dialogowe wysyłania ofert opcje uwierzytelniania. Wybierz **indywidualnych kont użytkowników** i kliknij przycisk **OK** aby powrócić do poprzedniego okna dialogowego.

    ![Okno dialogowe nowego projektu](identity/_static/03-new-project-auth.png)

    Wybieranie **indywidualnych kont użytkowników** kieruje Visual Studio do tworzenia modeli, ViewModels, widoki, kontrolery i inne zasoby wymagane do uwierzytelniania w ramach szablonu projektu.

    # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

    Jeśli używasz interfejsu wiersza polecenia platformy .NET Core, Utwórz nowy projekt za pomocą ``dotnet new mvc --auth Individual``. To polecenie tworzy nowy projekt z tego samego kodu szablonu tożsamości tworzonych w Visual Studio.

    Utworzony projekt zawiera `Microsoft.AspNetCore.Identity.EntityFrameworkCore` pakiet, który będzie się powtarzać, dane tożsamości i schematu przy użyciu programu SQL Server [Entity Framework Core](https://docs.microsoft.com/ef/).

    ---

2.  Konfigurowanie usługi tożsamości i Dodaj oprogramowanie pośredniczące w `Startup`.

    Usługi tożsamości są dodawane do aplikacji w `ConfigureServices` metoda `Startup` klasy:

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]
    
    Te usługi są udostępniane dla aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).
    
    Tożsamość jest włączone dla aplikacji przez wywołanie metody `UseAuthentication` w `Configure` metody. `UseAuthentication` dodaje uwierzytelniania [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) do potoku żądania.
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]
    
    Te usługi są udostępniane dla aplikacji za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection).
    
    Tożsamość jest włączone dla aplikacji przez wywołanie metody `UseIdentity` w `Configure` metody. `UseIdentity` dodaje plik cookie uwierzytelniania [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) do potoku żądania.
        
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]
    
    ---
     
    Aby uzyskać więcej informacji na temat uruchamiania aplikacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup).

3.  Utwórz użytkownika.

    Uruchom aplikację, a następnie kliknij polecenie **zarejestrować** łącza.

    Jeśli wykonujesz tę akcję po raz pierwszy, może być wymagany do uruchamiania migracji. Aplikacja wyświetli monit o **zastosować migracje**. Odśwież stronę, jeśli to konieczne.
    
    ![Zastosuj stronę sieci Web migracji](identity/_static/apply-migrations.png)
    
    Alternatywnie można testować przy użyciu ASP.NET Core Identity z aplikacji bez trwałego bazy danych przy użyciu bazy danych w pamięci. Aby użyć bazy danych w pamięci, należy dodać ``Microsoft.EntityFrameworkCore.InMemory`` pakiet do aplikacji i zmodyfikuj wywołanie aplikacji ``AddDbContext`` w ``ConfigureServices`` w następujący sposób:

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
    ```
    
    Po kliknięciu przez użytkownika **zarejestrować** łącza, ``Register`` akcji jest wywoływana na ``AccountController``. ``Register`` Akcja tworzy użytkownika, wywołując `CreateAsync` na `_userManager` obiektu (podano ``AccountController`` przez iniekcji zależności):
 
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

    Jeśli użytkownik został pomyślnie utworzony, użytkownik jest zalogowany przez wywołanie ``_signInManager.SignInAsync``.

    **Uwaga:** zobacz [konta potwierdzenia](xref:security/authentication/accconfirm#prevent-login-at-registration) kroki zapobiec bezpośredniego logowania podczas rejestracji.
 
4.  Zaloguj się.
 
    Użytkownicy mogą rejestrować klikając **Zaloguj** łącze u góry strony, lub mogą zostać przesłane do strony logowania, gdy próbują uzyskać dostępu do części witryny, która wymaga autoryzacji. Gdy użytkownik przesyła formularz na stronie logowania ``AccountController`` ``Login`` nosi nazwę akcji.

    ``Login`` Wywołania akcji ``PasswordSignInAsync`` na ``_signInManager`` obiektu (podano ``AccountController`` przez iniekcji zależności).

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]
 
    Podstawowym ``Controller`` klasy ujawnia ``User`` właściwości, którego można korzystać z metod kontrolera. Na przykład można wyliczyć `User.Claims` i podejmowania decyzji dotyczących autoryzacji. Aby uzyskać więcej informacji, zobacz [autoryzacji](xref:security/authorization/index).
 
5.  Wyloguj się.
 
    Kliknięcie przycisku **Wyloguj się** link wywołania `LogOut` akcji.
 
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]
 
    Poprzedni kod powyżej wywołania `_signInManager.SignOutAsync` metody. `SignOutAsync` Metody czyści oświadczeń użytkownika przechowywane w pliku cookie.
 
<a name="pw"></a>
6.  Konfiguracja.

    Tożsamość ma niektóre domyślne zachowania, które mogą zostać zastąpione w klasie uruchomienia aplikacji. `IdentityOptions` Nie można skonfigurować, korzystając z domyślnego zachowania. Poniższy kod ustawia kilka opcji siły hasła:

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

    ---
    
    Aby uzyskać więcej informacji na temat konfigurowania tożsamości, zobacz [konfigurowania tożsamości](xref:security/authentication/identity-configuration).
    
    Można także skonfigurować typ danych klucza podstawowego, zobacz [typu danych kluczy podstawowych konfigurowania tożsamości](xref:security/authentication/identity-primary-key-configuration).
 
7.  Wyświetl bazy danych.

    Jeśli aplikacja używa bazy danych programu SQL Server (domyślnie w systemie Windows, jak i dla użytkowników programu Visual Studio), można wyświetlić baza danych utworzona aplikacja. Można użyć **programu SQL Server Management Studio**. W programie Visual Studio, wybierz opcję **widoku** > **Eksplorator obiektów SQL Server**. Połączyć się z **(localdb) \MSSQLLocalDB**. Baza danych o nazwie odpowiadającej **aspnet — <*Nazwa projektu*>-<*Data ciąg* >**  jest wyświetlany.

    ![Menu kontekstowe w tabeli bazy danych AspNetUsers](identity/_static/04-db.png)
    
    Rozwiń bazę danych i jego **tabel**, kliknij prawym przyciskiem myszy **dbo. AspNetUsers** tabeli i wybierz **danych widoku**.

8. Weryfikowanie działania tożsamości

    Wartość domyślna *aplikacji sieci Web platformy ASP.NET Core* szablon projektu umożliwia użytkownikom uzyskiwanie dostępu do żadnych czynności w aplikacji bez potrzeby logowania. Aby sprawdzić, czy działa tożsamości platformy ASP.NET, należy dodać`[Authorize]` atrybutu `About` akcji `Home` kontrolera.
 
    ```cs
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```
    
    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    Uruchom projekt za pomocą **Ctrl** + **F5** i przejdź do **o** strony. Tylko uwierzytelnieni użytkownicy mogą uzyskać dostępu do **o** strony, dlatego ASP.NET przekieruje Cię do strony logowania, aby zalogować się lub zarejestrować.

    # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

    Otwórz okno polecenia i przejdź do katalogu głównego projektu zawierającego katalogu `.csproj` pliku. Uruchom [dotnet Uruchom](/dotnet/core/tools/dotnet-run) polecenie do uruchomienia aplikacji:

    ```cs
    dotnet run 
    ```

    Przeglądaj adres URL określony w danych wyjściowych z [dotnet Uruchom](/dotnet/core/tools/dotnet-run) polecenia. Ten adres URL powinien wskazywać `localhost` z numeru portu wygenerowany. Przejdź do **o** strony. Tylko uwierzytelnieni użytkownicy mogą uzyskać dostępu do **o** strony, dlatego ASP.NET przekieruje Cię do strony logowania, aby zalogować się lub zarejestrować.

    ---

## <a name="identity-components"></a>Składniki tożsamości

Zestaw odwołania podstawowego dla systemu tożsamości jest `Microsoft.AspNetCore.Identity`. Ten pakiet zawiera podstawowy zestaw interfejsów dla platformy ASP.NET Core tożsamości i jest dołączony przez `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Te zależności są niezbędne do używania systemu tożsamości w aplikacji platformy ASP.NET Core:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Zawiera typy wymaganych do korzystania z tożsamości z programu Entity Framework Core.

* `Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core jest technologii dostępu do danych zalecane przez firmę Microsoft relacyjnych baz danych, takich jak SQL Server. Do testowania, można użyć `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies` -Oprogramowanie pośredniczące, które umożliwia aplikacji korzystanie z uwierzytelniania opartego na pliku cookie.

## <a name="migrating-to-aspnet-core-identity"></a>Migrowanie tożsamości platformy ASP.NET Core

Aby uzyskać dodatkowe informacje i wskazówki dotyczące migrowania istniejących tożsamości przechowywania można znaleźć [Migrowanie uwierzytelnianie i tożsamość](xref:migration/identity).

## <a name="setting-password-strength"></a>Ustawianie siły hasła

Zobacz [konfiguracji](#pw) dla przykładu, która ustawia wymagania minimalnej hasła.

## <a name="next-steps"></a>Następne kroki

* [Migrowanie uwierzytelnianie i tożsamość](xref:migration/identity)
* [Potwierdzenie konta i odzyskiwanie hasła](xref:security/authentication/accconfirm)
* [Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS](xref:security/authentication/2fa)
* [Facebook, Google i zewnętrznego dostawcy uwierzytelniania](xref:security/authentication/social/index)
