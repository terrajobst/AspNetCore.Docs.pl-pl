---
title: Korzystanie z języka SQL w aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dowiedz się więcej o używaniu SQL Server LocalDB lub oprogramowania SQLite w aplikacji ASP.NET Core MVC.
ms.author: riande
ms.date: 8/16/2019
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: de392f4220cf0182d02a20f387164d2f4b184b58
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2019
ms.locfileid: "72289077"
---
# <a name="work-with-sql-in-aspnet-core"></a>Współpraca z SQL w ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

Obiekt `MvcMovieContext` obsługuje zadanie łączenia się z bazą danych i mapowania obiektów `Movie` do rekordów bazy danych. Kontekst bazy danych jest zarejestrowany z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w metodzie `ConfigureServices` w pliku *Startup.cs* :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

System [konfiguracji](xref:fundamentals/configuration/index) ASP.NET Core odczytuje `ConnectionString`. W przypadku lokalnego projektowania pobiera parametry połączenia z pliku *appSettings. JSON* :

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=5-6)]

System [konfiguracji](xref:fundamentals/configuration/index) ASP.NET Core odczytuje `ConnectionString`. W przypadku lokalnego projektowania pobiera parametry połączenia z pliku *appSettings. JSON* :

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---

Gdy aplikacja jest wdrażana na serwerze testowym lub produkcyjnym, zmienna środowiskowa może służyć do ustawiania parametrów połączenia do SQL Server produkcyjnej. Aby uzyskać więcej informacji, zobacz [Konfiguracja](xref:fundamentals/configuration/index) .

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB to uproszczona wersja aparatu bazy danych SQL Server Express, która jest przeznaczona do tworzenia programów. LocalDB uruchamia się na żądanie i działa w trybie użytkownika, więc nie istnieje złożona konfiguracja. Domyślnie baza danych LocalDB tworzy pliki *MDF* w katalogu *C:/Users/{User}* .

* Z menu **Widok** Otwórz **Eksplorator obiektów SQL Server** (SSOX).

  ![Menu Widok](working-with-sql/_static/ssox.png)

* Kliknij prawym przyciskiem myszy tabelę `Movie` **> widok projektanta**

  ![Menu kontekstowe jest otwarte w tabeli filmów](working-with-sql/_static/design.png)

  ![Tabela filmów otwarta w projektancie](working-with-sql/_static/dv.png)

Zwróć uwagę na ikonę klucza obok `ID`. Domyślnie, EF wprowadzi właściwość o nazwie `ID` klucz podstawowy.

* Kliknij prawym przyciskiem myszy tabelę `Movie` **> Wyświetl dane**

  ![Menu kontekstowe jest otwarte w tabeli filmów](working-with-sql/_static/ssox2.png)

  ![Otwórz tabelę filmów pokazującą dane tabeli](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---
<!-- End of VS tabs -->

## <a name="seed-the-database"></a>Wypełnianie bazy danych

Utwórz nową klasę o nazwie `SeedData` w folderze *modele* . Zastąp wygenerowany kod następującym:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/SeedData.cs?name=snippet_1)]

Jeśli w bazie danych znajdują się jakiekolwiek filmy, inicjatora inicjatora zwraca i nie dodano żadnych filmów.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a>Dodawanie inicjatora inicjatora

Zastąp zawartość *program.cs* następującym kodem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Program.cs)]

Testowanie aplikacji

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Usuń wszystkie rekordy z bazy danych. Można to zrobić za pomocą linków usuwania w przeglądarce lub z SSOX.
* Wymuś inicjalizację aplikacji (wywołaj metody z klasy `Startup`), aby Metoda inicjatora była uruchomiona. Aby wymusić inicjalizację, IIS Express należy zatrzymać i uruchomić ponownie. Można to zrobić przy użyciu następujących metod:

  * Kliknij prawym przyciskiem myszy ikonę IIS Express pasku zadań w obszarze powiadomień i naciśnij pozycję **Zakończ** lub **Zatrzymaj witrynę**

    ![Ikona paska zadań IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](working-with-sql/_static/stopIIS.png)

    * W przypadku uruchamiania programu VS w trybie innym niż debugowanie naciśnij klawisz F5, aby uruchomić w trybie debugowania
    * W przypadku uruchamiania programu VS w trybie debugowania Zatrzymaj debuger i naciśnij klawisz F5.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

Usuń wszystkie rekordy z bazy danych (w związku z czym zostanie uruchomiona Metoda inicjatora). Zatrzymaj i uruchom aplikację, aby wypełniać bazę danych.

---

Aplikacja pokazuje dane z rozrzutu.

![Aplikacja filmowa MVC otwarta w przeglądarce Microsoft Edge pokazująca dane filmu](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> [Poprzedni](adding-model.md)
> [dalej](controller-methods-views.md)
::: moniker-end

::: moniker range="< aspnetcore-3.0"

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

Obiekt `MvcMovieContext` obsługuje zadanie łączenia się z bazą danych i mapowania obiektów `Movie` do rekordów bazy danych. Kontekst bazy danych jest zarejestrowany z kontenerem [iniekcji zależności](xref:fundamentals/dependency-injection) w metodzie `ConfigureServices` w pliku *Startup.cs* :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

System [konfiguracji](xref:fundamentals/configuration/index) ASP.NET Core odczytuje `ConnectionString`. W przypadku lokalnego projektowania pobiera parametry połączenia z pliku *appSettings. JSON* :

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

System [konfiguracji](xref:fundamentals/configuration/index) ASP.NET Core odczytuje `ConnectionString`. W przypadku lokalnego projektowania pobiera parametry połączenia z pliku *appSettings. JSON* :

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---

Podczas wdrażania aplikacji na serwerze testowym lub produkcyjnym można użyć zmiennej środowiskowej lub innego podejścia do ustawiania parametrów połączenia dla rzeczywistych SQL Server. Aby uzyskać więcej informacji, zobacz [Konfiguracja](xref:fundamentals/configuration/index) .

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB to uproszczona wersja aparatu bazy danych SQL Server Express, która jest przeznaczona do tworzenia programów. LocalDB uruchamia się na żądanie i działa w trybie użytkownika, więc nie istnieje złożona konfiguracja. Domyślnie baza danych LocalDB tworzy pliki *MDF* w katalogu *C:/Users/{User}* .

* Z menu **Widok** Otwórz **Eksplorator obiektów SQL Server** (SSOX).

  ![Menu Widok](working-with-sql/_static/ssox.png)

* Kliknij prawym przyciskiem myszy tabelę `Movie` **> widok projektanta**

  ![Menu kontekstowe jest otwarte w tabeli filmów](working-with-sql/_static/design.png)

  ![Tabela filmów otwarta w projektancie](working-with-sql/_static/dv.png)

Zwróć uwagę na ikonę klucza obok `ID`. Domyślnie, EF wprowadzi właściwość o nazwie `ID` klucz podstawowy.

* Kliknij prawym przyciskiem myszy tabelę `Movie` **> Wyświetl dane**

  ![Menu kontekstowe jest otwarte w tabeli filmów](working-with-sql/_static/ssox2.png)

  ![Otwórz tabelę filmów pokazującą dane tabeli](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---
<!-- End of VS tabs -->

## <a name="seed-the-database"></a>Wypełnianie bazy danych

Utwórz nową klasę o nazwie `SeedData` w folderze *modele* . Zastąp wygenerowany kod następującym:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/SeedData.cs?name=snippet_1)]

Jeśli w bazie danych znajdują się jakiekolwiek filmy, inicjatora inicjatora zwraca i nie dodano żadnych filmów.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a>Dodawanie inicjatora inicjatora

Zastąp zawartość *program.cs* następującym kodem:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Program.cs)]

Testowanie aplikacji

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Usuń wszystkie rekordy z bazy danych. Można to zrobić za pomocą linków usuwania w przeglądarce lub z SSOX.
* Wymuś inicjalizację aplikacji (wywołaj metody z klasy `Startup`), aby Metoda inicjatora była uruchomiona. Aby wymusić inicjalizację, IIS Express należy zatrzymać i uruchomić ponownie. Można to zrobić przy użyciu następujących metod:

  * Kliknij prawym przyciskiem myszy ikonę IIS Express pasku zadań w obszarze powiadomień i naciśnij pozycję **Zakończ** lub **Zatrzymaj witrynę**

    ![Ikona paska zadań IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](working-with-sql/_static/stopIIS.png)

    * W przypadku uruchamiania programu VS w trybie innym niż debugowanie naciśnij klawisz F5, aby uruchomić w trybie debugowania
    * W przypadku uruchamiania programu VS w trybie debugowania Zatrzymaj debuger i naciśnij klawisz F5.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio dla komputerów Mac](#tab/visual-studio-code+visual-studio-mac)

Usuń wszystkie rekordy z bazy danych (w związku z czym zostanie uruchomiona Metoda inicjatora). Zatrzymaj i uruchom aplikację, aby wypełniać bazę danych.

---

Aplikacja pokazuje dane z rozrzutu.

![Aplikacja filmowa MVC otwarta w przeglądarce Microsoft Edge pokazująca dane filmu](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> [Poprzedni](adding-model.md)
> [dalej](controller-methods-views.md)

::: moniker-end
