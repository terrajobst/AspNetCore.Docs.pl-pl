---
title: Praca z bazy danych LocalDB programu SQL Server i platformy ASP.NET Core
author: rick-anderson
description: W tym artykule wyjaśniono pracy z bazy danych LocalDB programu SQL Server i ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 1fd86fb61b7f5ddb760992ac10f9bee43ab831cf
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272146"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a>Praca z bazy danych LocalDB programu SQL Server i platformy ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette) 

`MovieContext` Obiektu obsługuje zadanie łączenia z bazą danych i mapowanie `Movie` obiektów do rekordów bazy danych. Kontekst bazy danych został zarejestrowany za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` metody w *Startup.cs* pliku:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

Aby uzyskać więcej informacji na temat metody używane w `ConfigureServices`, zobacz:

* [Obsługa interfejsów UE ogólne dane ochrony rozporządzenia (GDPR) w ASP.NET Core](xref:security/gdpr) dla `CookiePolicyOptions`.
* [SetCompatibilityVersion](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc)

::: moniker-end

Platformy ASP.NET Core [konfiguracji](xref:fundamentals/configuration/index) odczyty systemu `ConnectionString`. Lokalne działania projektowe, pobiera parametry połączenia z *appsettings.json* pliku. Wartość Nazwa bazy danych (`Database={Database name}`) mogą być inne dla wygenerowanego kodu. Wartość nazwy jest dowolnego.

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

Podczas wdrażania aplikacji na serwerze testowym lub produkcyjnym, można użyć zmiennej środowiskowej lub innej metody, aby ustawić parametry połączenia do rzeczywistego programu SQL Server. Zobacz [konfiguracji](xref:fundamentals/configuration/index) Aby uzyskać więcej informacji.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB.

To Zubożona wersja programu SQL Server Express aparat bazy danych jest przeznaczony do tworzenia aplikacji programu. LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonych konfiguracji. Domyślnie tworzy bazy danych LocalDB "\*.mdf" plików *C:/Users/\<użytkownika\>*  katalogu.

<a name="ssox"></a>
* Z **widoku** menu, otwórz **Eksplorator obiektów SQL Server** (SSOX).

  ![Menu Widok](sql/_static/ssox.png)

* Kliknij prawym przyciskiem myszy `Movie` tabeli i wybierz **Widok projektanta**:

  ![Menu kontekstowe otwarty na tabeli film](sql/_static/design.png)

  ![Otwórz w projektancie tabeli film](sql/_static/dv.png)

Należy pamiętać ikonę klucza, obok pozycji `ID`. Domyślnie EF tworzy właściwość o nazwie `ID` klucza podstawowego.

* Kliknij prawym przyciskiem myszy `Movie` tabeli i wybierz **danych widoku**:

  ![Otwórz tabelę dane są wyświetlane tabeli film](sql/_static/vd22.png)

## <a name="seed-the-database"></a>Inicjatora bazy danych

Utwórz nową klasę o nazwie `SeedData` w *modele* folderu. Zastąp wygenerowany kod poniżej:

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

Jeśli w bazie danych są wszystkie filmy, zwraca inicjatora inicjatora i filmy nie zostaną dodane.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Dodaj inicjatora inicjatora

W *Program.cs*, zmodyfikuj `Main` metody wykonać następujące czynności:

* Pobierz wystąpienia kontekstu DB z kontenera iniekcji zależności.
* Wywołaj metodę inicjatora, przekazanie jej w kontekście.
* Po zakończeniu metody inicjatora, należy dysponować kontekstu.

Poniższy kod przedstawia zaktualizowanego *Program.cs* pliku.

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]

::: moniker-end

Nie wywołać aplikacji produkcyjnej `Database.Migrate`. Jest ona dodawana do kodu poprzedzających, aby zapobiec następujący wyjątek podczas `Update-Database` nie zostało uruchomione:

SqlException: Nie można otworzyć bazy danych "RazorPagesMovieContext-21" żądanego podczas logowania. Logowanie nie powiodło się.
Użytkownik "Nazwa użytkownika" Logowanie nie powiodło się.

### <a name="test-the-app"></a>Testowanie aplikacji

* Usuń wszystkie rekordy w bazie danych. Można to zrobić z łączami Usuń w przeglądarce lub z [SSOX](xref:tutorials/razor-pages/new-field#ssox)
* Wymusić na aplikacji, aby zainicjować (wywołanie metody w `Startup` klasy), uruchamia seed — metoda. Aby wymusić inicjowania, usługi IIS Express musi zostać zatrzymana i uruchomiona ponownie. Można to zrobić za pomocą dowolnego z następujących metod:

  * Kliknij prawym przyciskiem myszy ikonę na pasku zadań systemu usług IIS Express w obszarze powiadomień, a następnie wybierz polecenie **zakończenia** lub **Zatrzymaj witrynę**:

    ![Usługi IIS Express ikony paska zadań](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](sql/_static/stopIIS.png)

    * Podczas uruchamiania programu VS w trybie bez debugowania, naciśnij klawisz F5, aby uruchomić w trybie debugowania.
    * W przypadku korzystania z wersji programu VS w trybie debugowania, zatrzymaniu debugera i naciśnij klawisz F5.
   
Aplikacja zawiera wprowadzonych danych:

![Otwórz w przeglądarce Chrome danych Film przedstawiający aplikacji film](sql/_static/m55.png)

Następny samouczek spowoduje oczyszczenie prezentacji danych.

> [!div class="step-by-step"]
> [Poprzedni: Tworzenie szkieletu stron Razor](xref:tutorials/razor-pages/page)
> [dalej: aktualizowanie stron](xref:tutorials/razor-pages/da1)
