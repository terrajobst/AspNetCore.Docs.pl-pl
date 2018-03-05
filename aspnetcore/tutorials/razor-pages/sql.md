---
title: Praca z bazy danych LocalDB programu SQL Server i platformy ASP.NET Core
author: rick-anderson
description: "W tym artykule wyjaśniono pracy z bazy danych LocalDB programu SQL Server i ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 408072fbe83eadb0ae3c35c2789bda8ecaac58c0
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="working-with-sql-server-localdb-and-aspnet-core"></a>Praca z bazy danych LocalDB programu SQL Server i platformy ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Audette Jan](https://twitter.com/joeaudette) 

`MovieContext` Obiektu obsługuje zadanie łączenia z bazą danych i mapowanie `Movie` obiektów do rekordów bazy danych. Kontekst bazy danych został zarejestrowany za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` metody w *Startup.cs* pliku:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

Platformy ASP.NET Core [konfiguracji](xref:fundamentals/configuration/index) odczyty systemu `ConnectionString`. Lokalne działania projektowe, pobiera parametry połączenia z *appsettings.json* pliku:

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

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

Jeśli w bazie danych są wszystkie filmy, zwraca inicjatora inicjatora i filmy nie zostaną dodane.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Dodaj inicjatora inicjatora

Dodaj inicjatora inicjatora na końcu `Main` metody w *Program.cs* pliku:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

Testowanie aplikacji

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

>[!div class="step-by-step"]
[Poprzedni: Tworzenie szkieletu stron Razor](xref:tutorials/razor-pages/page)
[dalej: aktualizowanie stron](xref:tutorials/razor-pages/da1)
