---
title: Praca z programu SQL Server LocalDB i ASP.NET Core
author: rick-anderson
description: W tym artykule wyjaśniono, pracy z programu SQL Server LocalDB i ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 255faf12064aa424d51fb6faa801884c474bd288
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889486"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a>Praca z programu SQL Server LocalDB i ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette) 

`MovieContext` Obiektu obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych. Kontekst bazy danych jest zarejestrowany w [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` method in Class metoda *Startup.cs* pliku:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

Aby uzyskać więcej informacji na temat metod używanych w `ConfigureServices`, zobacz:

* [Obsługa Unii Europejskiej ogólnego danych (GDPR Protection Regulation) w programie ASP.NET Core](xref:security/gdpr) dla `CookiePolicyOptions`.
* [SetCompatibilityVersion](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc)

::: moniker-end

ASP.NET Core [konfiguracji](xref:fundamentals/configuration/index) odczyty system `ConnectionString`. Dla wdrożenia lokalnego, pobiera parametry połączenia z *appsettings.json* pliku. Wartość nazwy bazy danych (`Database={Database name}`) będzie różna dla wygenerowanego kodu. Wartość nazwy jest dowolnego.

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

Podczas wdrażania aplikacji na serwerze testowym lub produkcyjnym, można użyć zmiennej środowiskowej lub innej metody, które można ustawić parametrów połączenia do rzeczywistego programu SQL Server. Zobacz [konfiguracji](xref:fundamentals/configuration/index) Aby uzyskać więcej informacji.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB to Uproszczona wersja aparatu programu SQL Server Express bazy danych, która jest przeznaczona do tworzenia programu. LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji. Domyślnie tworzy bazy danych LocalDB "\*.mdf" pliki *C:/Users/\<użytkownika\>*  katalogu.

<a name="ssox"></a>
* Z **widoku** menu Otwórz **Eksplorator obiektów SQL Server** (SSOX).

  ![Menu Widok](sql/_static/ssox.png)

* Kliknij prawym przyciskiem myszy `Movie` tabeli, a następnie wybierz pozycję **Projektant widoków**:

  ![Menu kontekstowe jest otwarte w tabeli filmu](sql/_static/design.png)

  ![Otwórz w Projektancie tabel filmu](sql/_static/dv.png)

Należy zauważyć ikonę klucza, obok `ID`. Domyślnie program EF tworzy właściwość o nazwie `ID` dla klucza podstawowego.

* Kliknij prawym przyciskiem myszy `Movie` tabeli, a następnie wybierz pozycję **dane widoku**:

  ![Tabela filmu otwarte, wyświetlanie tabeli danych](sql/_static/vd22.png)

## <a name="seed-the-database"></a>Inicjowanie bazy danych

Utwórz nową klasę o nazwie `SeedData` w *modeli* folderu. Zastąp wygenerowany kod następujących czynności:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

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

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]

::: moniker-end

Nie może wywołać aplikacji produkcyjnej `Database.Migrate`. Jest ona dodawana do poprzedniego kodu, aby zapobiec następujący wyjątek podczas `Update-Database` nie zostały uruchomione:

Sqlexception —: Nie można otworzyć bazy danych "RazorPagesMovieContext-21" żądanego podczas logowania. Logowanie nie powiodło się.
Nie można zalogować użytkownika "Nazwa użytkownika".

### <a name="test-the-app"></a>Testowanie aplikacji

* Usuń wszystkie rekordy w bazie danych. Można to zrobić za pomocą łącza delete w przeglądarce, albo z [SSOX](xref:tutorials/razor-pages/new-field#ssox)
* Wymusić na aplikacji, aby zainicjować (wywoływanie metody w `Startup` klasy), uruchamia seed — metoda. Aby wymusić inicjowania, usługi IIS Express musi zostać zatrzymana i uruchomiona ponownie. To zrobić przy użyciu dowolnej z następujących metod:

  * Kliknij prawym przyciskiem myszy ikonę na pasku zadań systemu usług IIS Express w obszarze powiadomień, a następnie naciśnij pozycję **zakończenia** lub **zatrzymać witrynę**:

    ![Usługi IIS Express ikony na pasku zadań](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](sql/_static/stopIIS.png)

    * Podczas uruchamiania w trybie bez debugowania programu VS, naciśnij klawisz F5, aby uruchomić w trybie debugowania.
    * Podczas uruchamiania programu VS w trybie debugowania, Zatrzymaj debuger, a następnie naciśnij klawisz F5.
   
Aplikacja przedstawiono wprowadzonych danych:

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji filmów](sql/_static/m55.png)

Następny samouczek spowoduje oczyszczenie prezentacji danych.

> [!div class="step-by-step"]
> [Poprzedni: Działanie stron Razor](xref:tutorials/razor-pages/page)
> [dalej: aktualizowanie stron](xref:tutorials/razor-pages/da1)
