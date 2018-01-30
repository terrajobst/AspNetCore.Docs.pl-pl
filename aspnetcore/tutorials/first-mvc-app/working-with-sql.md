---
title: Praca z bazy danych LocalDB programu SQL Server
author: rick-anderson
description: "Przy użyciu bazy danych LocalDB programu SQL Server z prostej aplikacji MVC"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: a0aa6fdfa51650628021a4ba6d0533e7e0e39200
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="working-with-sql-server-localdb"></a>Praca z bazy danych LocalDB programu SQL Server

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

`MvcMovieContext` Obiektu obsługuje zadanie łączenia z bazą danych i mapowanie `Movie` obiektów do rekordów bazy danych. Kontekst bazy danych został zarejestrowany za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` metody w *Startup.cs* pliku:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

Platformy ASP.NET Core [konfiguracji](xref:fundamentals/configuration/index) odczyty systemu `ConnectionString`. Lokalne działania projektowe, pobiera parametry połączenia z *appsettings.json* pliku:

[!code-json[Main](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

Podczas wdrażania aplikacji na serwerze testowym lub produkcyjnym, można użyć zmiennej środowiskowej lub innej metody, aby ustawić parametry połączenia do rzeczywistego programu SQL Server. Zobacz [konfiguracji](xref:fundamentals/configuration/index) Aby uzyskać więcej informacji.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB.

To Zubożona wersja programu SQL Server Express aparat bazy danych jest przeznaczony do tworzenia aplikacji programu. LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonych konfiguracji. Domyślnie tworzy bazy danych LocalDB "\*.mdf" plików *C:/Users/\<użytkownika\>*  katalogu.

* Z **widoku** menu, otwórz **Eksplorator obiektów SQL Server** (SSOX).

  ![Menu Widok](working-with-sql/_static/ssox.png)

* Kliknij prawym przyciskiem myszy `Movie` tabeli **> Widok projektanta**

  ![Menu kontekstowe otwarty na tabeli film](working-with-sql/_static/design.png)

  ![Otwórz w projektancie tabeli film](working-with-sql/_static/dv.png)

Należy pamiętać ikonę klucza, obok pozycji `ID`. Domyślnie EF spowoduje, że właściwość o nazwie `ID` klucza podstawowego.

* Kliknij prawym przyciskiem myszy `Movie` tabeli **> Wyświetlanie danych**

  ![Menu kontekstowe otwarty na tabeli film](working-with-sql/_static/ssox2.png)

  ![Otwórz tabelę dane są wyświetlane tabeli film](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a>Inicjatora bazy danych

Utwórz nową klasę o nazwie `SeedData` w *modele* folderu. Zastąp wygenerowany kod poniżej:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

Jeśli w bazie danych są wszystkie filmy, zwraca inicjatora inicjatora i filmy nie zostaną dodane.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Dodaj inicjatora inicjatora

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Inicjator inicjatora, aby dodać `Main` metody w *Program.cs* pliku:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Dodaj inicjatora inicjatora na końcu `Configure` metody w *Startup.cs* pliku.

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---

Testowanie aplikacji

* Usuń wszystkie rekordy w bazie danych. Można to zrobić z łączami Usuń w przeglądarce lub SSOX.
* Wymusić na aplikacji, aby zainicjować (wywołanie metody w `Startup` klasy), uruchamia seed — metoda. Aby wymusić inicjowania, usługi IIS Express musi zostać zatrzymana i uruchomiona ponownie. Można to zrobić za pomocą dowolnego z następujących metod:

  * Kliknij prawym przyciskiem myszy ikonę na pasku zadań systemu usług IIS Express w obszarze powiadomień, a następnie wybierz polecenie **zakończenia** lub **zatrzymania lokacji**

    ![Usługi IIS Express ikony paska zadań](working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](working-with-sql/_static/stopIIS.png)

   * Podczas uruchamiania programu VS w trybie bez debugowania, naciśnij klawisz F5, aby uruchomić w trybie debugowania
   * W przypadku korzystania z wersji programu VS w trybie debugowania, zatrzymaniu debugera i naciśnij klawisz F5
   
Aplikacja zawiera wprowadzonych danych.

![Aplikacja MVC Movie otworzyć w programie Microsoft Edge danych Film przedstawiający](working-with-sql/_static/m55.png)

>[!div class="step-by-step"]
[Poprzednie](adding-model.md)
[dalej](controller-methods-views.md)  
