---
title: Praca z bazy danych i ASP.NET Core
author: rick-anderson
description: W tym artykule wyjaśniono, pracy z bazą danych i ASP.NET Core.
ms.author: riande
ms.date: 12/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 3e05f5dbc73c35f1f938346b2eaab8c0fa7d8ab9
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410300"
---
# <a name="work-with-a-database-and-aspnet-core"></a>Praca z bazy danych i ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette)

[!INCLUDE[](~/includes/rp/download.md)]

`RazorPagesMovieContext` Obiektu obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych. Kontekst bazy danych jest zarejestrowany w [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` method in Class metoda *Startup.cs*:

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---  
<!-- End of VS tabs -->

Aby uzyskać więcej informacji na temat metod używanych w `ConfigureServices`, zobacz:

* [Obsługa Unii Europejskiej ogólnego danych (GDPR Protection Regulation) w programie ASP.NET Core](xref:security/gdpr) dla `CookiePolicyOptions`.
* [SetCompatibilityVersion](xref:mvc/compatibility-version)

ASP.NET Core [konfiguracji](xref:fundamentals/configuration/index) odczyty system `ConnectionString`. Dla wdrożenia lokalnego, pobiera parametry połączenia z *appsettings.json* pliku.

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Wartość nazwy bazy danych (`Database={Database name}`) będzie różna dla wygenerowanego kodu. Wartość nazwy jest dowolnego.

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---  
<!-- End of VS tabs -->

Po wdrożeniu aplikacji na serwerze testowym lub produkcyjnym zmiennej środowiskowej można ustawić parametrów połączenia z serwerem bazy danych rzeczywistych. Zobacz [konfiguracji](xref:fundamentals/configuration/index) Aby uzyskać więcej informacji.

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB to Uproszczona wersja programu SQL Server Express aparatu bazy danych, która jest przeznaczona do tworzenia programu. LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji. Domyślnie tworzy bazy danych LocalDB `*.mdf` pliki `C:/Users/<user/>` katalogu.

<a name="ssox"></a>
* Z **widoku** menu Otwórz **Eksplorator obiektów SQL Server** (SSOX).

  ![Menu Widok](sql/_static/ssox.png)

* Kliknij prawym przyciskiem myszy `Movie` tabeli, a następnie wybierz pozycję **Projektant widoków**:

  ![Menu kontekstowe jest otwarte w tabeli filmu](sql/_static/design.png)

  ![Otwórz w Projektancie tabel filmu](sql/_static/dv.png)

Należy zauważyć ikonę klucza, obok `ID`. Domyślnie program EF tworzy właściwość o nazwie `ID` dla klucza podstawowego.

* Kliknij prawym przyciskiem myszy `Movie` tabeli, a następnie wybierz pozycję **dane widoku**:

  ![Tabela filmu otwarte, wyświetlanie tabeli danych](sql/_static/vd22.png)
<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]

---  
<!-- End of VS tabs -->

## <a name="seed-the-database"></a>Inicjowanie bazy danych

Utwórz nową klasę o nazwie `SeedData` w *modeli* folderu z następującym kodem:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

Czy istnieją wszystkie filmy w bazie danych, zwraca inicjatora inicjatora i filmy nie zostaną dodane.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Dodaj inicjator inicjatora

W *Program.cs*, zmodyfikuj `Main` metodę, aby wykonać następujące czynności:

* Pobierz wystąpienia kontekstu bazy danych z kontenera iniekcji zależności.
* Wywołaj metodę inicjatora, przekazując mu kontekst.
* Po zakończeniu seed — metoda, należy dysponować kontekstu.

Poniższy kod przedstawia zaktualizowanego *Program.cs* pliku.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

Nie może wywołać aplikacji produkcyjnej `Database.Migrate`. Jest ona dodawana do poprzedniego kodu, aby zapobiec następujący wyjątek podczas `Update-Database` nie zostały uruchomione:

SqlException: Nie można otworzyć bazy danych "RazorPagesMovieContext-21" żądanego podczas logowania. Logowanie nie powiodło się.
Nie można zalogować użytkownika "Nazwa użytkownika".

### <a name="test-the-app"></a>Testowanie aplikacji

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Usuń wszystkie rekordy w bazie danych. Można to zrobić za pomocą łącza delete w przeglądarce, albo z [SSOX](xref:tutorials/razor-pages/new-field#ssox)
* Wymusić na aplikacji, aby zainicjować (wywoływanie metody w `Startup` klasy), uruchamia seed — metoda. Aby wymusić inicjowania, usługi IIS Express musi zostać zatrzymana i uruchomiona ponownie. To zrobić przy użyciu dowolnej z następujących metod:

  * Kliknij prawym przyciskiem myszy ikonę na pasku zadań systemu usług IIS Express w obszarze powiadomień, a następnie naciśnij pozycję **zakończenia** lub **zatrzymać witrynę**:

    ![Usługi IIS Express ikony na pasku zadań](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](sql/_static/stopIIS.png)

    * Podczas uruchamiania w trybie bez debugowania programu VS, naciśnij klawisz F5, aby uruchomić w trybie debugowania.
    * Podczas uruchamiania programu VS w trybie debugowania, Zatrzymaj debuger, a następnie naciśnij klawisz F5.

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

(Aby uruchomi seed — metoda), należy usunąć wszystkie rekordy w bazie danych. Zatrzymywanie i uruchamianie aplikacji w celu umieszczenia bazy danych.

Aplikacja zawiera wprowadzonych danych.

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

(Aby uruchomi seed — metoda), należy usunąć wszystkie rekordy w bazie danych. Zatrzymywanie i uruchamianie aplikacji w celu umieszczenia bazy danych.

Aplikacja zawiera wprowadzonych danych.

---  
<!-- End of VS tabs -->


   
Aplikacja przedstawiono wprowadzonych danych:

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji filmów](sql/_static/m55.png)

Następny samouczek spowoduje oczyszczenie prezentacji danych.

> [!div class="step-by-step"]
> [Poprzednie: Działanie stron Razor](xref:tutorials/razor-pages/page)
> [dalej: Aktualizowanie stron](xref:tutorials/razor-pages/da1)
