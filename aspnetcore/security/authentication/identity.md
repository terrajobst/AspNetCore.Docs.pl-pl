---
title: Wprowadzenie do tożsamości programu ASP.NET Core
author: rick-anderson
description: Tożsamość za pomocą aplikacji ASP.NET Core. Zawiera wymagania dotyczące hasła ustawienie (RequireDigit, RequiredLength, RequiredUniqueChars i inne).
ms.author: riande
ms.date: 01/24/2018
uid: security/authentication/identity
ms.openlocfilehash: c231a7619a4433ce004342ce68564e4c3892e702
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829305"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>Wprowadzenie do tożsamości programu ASP.NET Core

Przez [autorem jest Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), Galloway'em Jon [Erik Reitan](https://github.com/Erikre), i [Steve Smith](https://ardalis.com/)

Tożsamość platformy ASP.NET Core jest systemu członkostwa, co pozwala na dodawanie funkcji logowania do aplikacji. Użytkownicy mogą tworzyć konta usługi i zaloguj się przy użyciu nazwy użytkownika i hasło lub można użyć dostawcy logowania zewnętrznego, takich jak Facebook, Google, Microsoft Account, Twitter lub inne osoby.

Można skonfigurować tożsamości platformy ASP.NET Core używać bazy danych programu SQL Server do przechowywania nazwy użytkownika, hasła i dane profilu. Alternatywnie można użyć własnego magazynu trwałego na przykład Azure Table Storage. Ten dokument zawiera instrukcje dotyczące programu Visual Studio, a także uzyskać za pomocą interfejsu wiersza polecenia.

[Wyświetlanie lub pobieranie przykładowego kodu.](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(Jak pobrać)](xref:tutorials/index#how-to-download-a-sample)

## <a name="overview-of-identity"></a>Przegląd tożsamości

W tym temacie będziesz Dowiedz się, jak dodać funkcje, aby zarejestrować, zaloguj się za pomocą tożsamości platformy ASP.NET Core i wylogowania użytkownika. Aby uzyskać bardziej szczegółowe instrukcje dotyczące tworzenia aplikacji za pomocą tożsamości platformy ASP.NET Core zobacz sekcję następne kroki na końcu tego artykułu.

1. Tworzenie projektu aplikacji sieci Web programu ASP.NET Core z indywidualnymi kontami użytkowników.

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   W programie Visual Studio, wybierz **pliku** > **New** > **projektu**. Wybierz **aplikacji sieci Web programu ASP.NET Core** i kliknij przycisk **OK**.

   ![Okno dialogowe nowego projektu](identity/_static/01-new-project.png)

   Wybierz platformy ASP.NET Core **aplikacji sieci Web (Model-View-Controller)** dla platformy ASP.NET Core 2.x, a następnie wybierz **Zmień uwierzytelnianie**.

   ![Okno dialogowe nowego projektu](identity/_static/02-new-project.png)

   Zostanie wyświetlone okno dialogowe oferty opcje uwierzytelniania. Wybierz **indywidualne konta użytkowników** i kliknij przycisk **OK** aby powrócić do poprzedniego okna dialogowego.

   ![Okno dialogowe nowego projektu](identity/_static/03-new-project-auth.png)

   Wybieranie **indywidualne konta użytkowników** kieruje programu Visual Studio do tworzenia modeli, modele widoków, widoki, kontrolery i innych zasobów wymaganych do uwierzytelniania w ramach szablonu projektu.

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

   Jeśli używasz interfejsu wiersza polecenia platformy .NET Core, Utwórz nowy projekt za pomocą `dotnet new mvc --auth Individual`. To polecenie tworzy nowy projekt za pomocą tego samego kodu szablonu tożsamości, tworzonych w programie Visual Studio.

   Utworzono projekt zawiera `Microsoft.AspNetCore.Identity.EntityFrameworkCore` pakiet, który będzie się powtarzać, dane tożsamości i schematów do programu SQL Server przy użyciu [Entity Framework Core](https://docs.microsoft.com/ef/).

   ---

2. Konfigurowanie usługi zarządzania tożsamościami i Dodaj oprogramowanie pośredniczące w `Startup`.

   Usługi tożsamości są dodawane do aplikacji w `ConfigureServices` method in Class metoda `Startup` klasy:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   Te usługi są dostępne dla aplikacji za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).

   Tożsamość jest włączone dla aplikacji, wywołując `UseAuthentication` w `Configure` metody. `UseAuthentication` dodaje uwierzytelniania [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) do potoku żądania.

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   Te usługi są dostępne dla aplikacji za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection).

   Tożsamość jest włączone dla aplikacji, wywołując `UseIdentity` w `Configure` metody. `UseIdentity` dodaje na podstawie plików cookie uwierzytelniania [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) do potoku żądania.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

   ---

   Aby uzyskać więcej informacji na temat uruchamiania aplikacji, zobacz [uruchamiania aplikacji](xref:fundamentals/startup).

3. Utwórz użytkownika.

   Uruchom aplikację, a następnie kliknij polecenie **zarejestrować** łącza.

   Jeśli wykonujesz tę akcję po raz pierwszy, może być wymagane do uruchamiania migracji. Aplikacja wyświetli monit o **zastosować migracje**. Jeśli to konieczne, należy odświeżyć stronę.

   ![Zastosuj migracje strony sieci Web](identity/_static/apply-migrations.png)

   Alternatywnie można przetestować za pomocą tożsamości platformy ASP.NET Core z aplikacją, bez trwałego bazy danych przy użyciu bazy danych w pamięci. Aby użyć bazy danych w pamięci, należy dodać `Microsoft.EntityFrameworkCore.InMemory` pakietu z aplikacją i zmodyfikuj wywołanie aplikacji `AddDbContext` w `ConfigureServices` w następujący sposób:

   ```csharp
   services.AddDbContext<ApplicationDbContext>(options =>
       options.UseInMemoryDatabase(Guid.NewGuid().ToString()));
   ```

   Kiedy użytkownik kliknie **zarejestrować** łącza, `Register` jest wywoływana Akcja `AccountController`. `Register` Akcja powoduje utworzenie użytkownika, wywołując `CreateAsync` na `_userManager` obiektu (udostępniane `AccountController` przez wstrzykiwanie zależności):

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

   Jeśli użytkownik został pomyślnie utworzony, użytkownik jest zalogowany przez wywołanie `_signInManager.SignInAsync`.

   **Uwaga:** zobacz [konta potwierdzenia](xref:security/authentication/accconfirm#prevent-login-at-registration) kroki uniknąć natychmiastowego logowania podczas rejestracji.

4. Zaloguj się.

   Użytkownicy mogą się logować, klikając **Zaloguj** link u góry strony, lub może być nastąpi przejście do strony logowania, gdy próbują uzyskać dostęp do części witryny, która wymaga autoryzacji. Gdy użytkownik przesyła formularz na stronie logowania `AccountController` `Login` nosi nazwę akcji.

   `Login` Wywołania akcji `PasswordSignInAsync` na `_signInManager` obiektu (udostępniane `AccountController` przez wstrzykiwanie zależności).

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

   Podstawa `Controller` klasy ujawnia `User` właściwość, której będziesz mieć dostęp z metody kontrolera. Na przykład, można wyliczyć `User.Claims` i podejmowania decyzji dotyczących autoryzacji. Aby uzyskać więcej informacji, zobacz [autoryzacji](xref:security/authorization/index).

5. Zaloguj.

   Klikając **Wyloguj** link wywołania `LogOut` akcji.

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   Powyższy kod powyżej wywołania `_signInManager.SignOutAsync` metody. `SignOutAsync` Metoda czyści oświadczenia użytkownika przechowywane w pliku cookie.

<a name="pw"></a>
6. Konfiguracja.

   Tożsamość ma niektóre zachowania domyślne, które mogą zostać zastąpione w klasie uruchamiania aplikacji. `IdentityOptions` nie należy skonfigurować, korzystając z zachowania domyślnego. Poniższy kod ustawia kilka opcji siły hasła:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=13-33)]

   ---

   Aby uzyskać więcej informacji o sposobie konfigurowania tożsamości, zobacz [konfigurowania tożsamości](xref:security/authentication/identity-configuration).

   Można także skonfigurować typ danych klucza podstawowego, zobacz [konfigurowania tożsamości kluczy podstawowych danych typu](xref:security/authentication/identity-primary-key-configuration).

7. Wyświetl bazy danych.

   Jeśli aplikacja używa bazy danych programu SQL Server (ustawienie domyślne dla Windows i dla użytkowników programu Visual Studio), możesz wyświetlić aplikacja utworzona w bazie danych. Możesz użyć **SQL Server Management Studio**. W programie Visual Studio, wybierz opcję **widoku** > **Eksplorator obiektów SQL Server**. Połączyć się z **(localdb) \MSSQLLocalDB**. Bazy danych z nazwą pasującą `aspnet-<name of your project>-<guid>` jest wyświetlana.

   ![Menu kontekstowe w tabeli bazy danych AspNetUsers](identity/_static/04-db.png)

   Rozwiń bazę danych i jego **tabel**, kliknij prawym przyciskiem myszy **dbo. AspNetUsers** tabeli, a następnie wybierz pozycję **dane widoku**.

8. Sprawdź, czy działa tożsamości

    Wartość domyślna *aplikacji sieci Web programu ASP.NET Core* szablon projektu umożliwia użytkownikom uzyskiwanie dostępu do żadnych działań w aplikacji bez konieczności do logowania. Aby sprawdzić, czy produktu ASP.NET Identity działa, Dodaj`[Authorize]` atrybutu `About` akcji `Home` kontrolera.

    ```csharp
    [Authorize]
    public IActionResult About()
    {
        ViewData["Message"] = "Your application description page.";
        return View();
    }
    ```

    # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

    Uruchom projekt za pomocą **Ctrl** + **F5** i przejdź do **o** strony. Tylko uwierzytelnieni użytkownicy mogą uzyskiwać dostęp do **o** teraz strony ASP.NET przekieruje Cię do strony logowania, aby zalogować się lub zarejestruj.

    # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

    Otwórz okno polecenia i przejdź do katalogu głównego projektu katalogu zawierającego `.csproj` pliku. Uruchom [dotnet, uruchom](/dotnet/core/tools/dotnet-run) polecenia do uruchomienia aplikacji:

    ```csharp
    dotnet run 
    ```

    Przejdź do adresu URL określonego w danych wyjściowych [dotnet, uruchom](/dotnet/core/tools/dotnet-run) polecenia. Adres URL powinien wskazywać `localhost` za pomocą wygenerowanego numeru portu. Przejdź do **o** strony. Tylko uwierzytelnieni użytkownicy mogą uzyskiwać dostęp do **o** teraz strony ASP.NET przekieruje Cię do strony logowania, aby zalogować się lub zarejestruj.

    ---

## <a name="identity-components"></a>Składniki tożsamości

Zestaw odwołanie podstawowe dla systemu tożsamości jest `Microsoft.AspNetCore.Identity`. Ten pakiet zawiera podstawowy zestaw interfejsów dla produktu ASP.NET Core Identity i jest dołączony przez `Microsoft.AspNetCore.Identity.EntityFrameworkCore`.

Te zależności są niezbędne do używania systemu tożsamości w aplikacji platformy ASP.NET Core:

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore` -Zawiera wymaganych typów tożsamości za pomocą platformy Entity Framework Core.

* `Microsoft.EntityFrameworkCore.SqlServer` -Entity Framework Core jest technologii dostępu do danych zalecane przez firmę Microsoft dla relacyjnych baz danych, takich jak program SQL Server. W przypadku testowania można użyć `Microsoft.EntityFrameworkCore.InMemory`.

* `Microsoft.AspNetCore.Authentication.Cookies` -Oprogramowania pośredniczącego, które umożliwia aplikacji korzystanie z uwierzytelniania na podstawie plików cookie.

## <a name="migrating-to-aspnet-core-identity"></a>Migrowanie tożsamości platformy ASP.NET Core

Dla dodatkowe informacje i wskazówki dotyczące migrowania istniejących tożsamości ze sklepu zobacz [migracji uwierzytelnianie i tożsamość](xref:migration/identity).

## <a name="setting-password-strength"></a>Ustawianie siły hasła

Zobacz [konfiguracji](#pw) przykład określająca wymagania dotyczące minimalnych hasła.

## <a name="next-steps"></a>Następne kroki

* [Migracji, uwierzytelnianie i tożsamość](xref:migration/identity)
* [Potwierdzenie konta i odzyskiwanie hasła](xref:security/authentication/accconfirm)
* [Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS](xref:security/authentication/2fa)
* [Facebook, Google i zewnętrznego dostawcy uwierzytelniania](xref:security/authentication/social/index)
