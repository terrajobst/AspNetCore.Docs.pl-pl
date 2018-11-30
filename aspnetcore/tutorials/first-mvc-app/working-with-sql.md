---
title: Praca z programu SQL Server LocalDB w aplikacji MVC platformy ASP.NET Core
author: rick-anderson
description: Dowiedz się więcej o korzystaniu z programu SQL Server LocalDB w prostej aplikacji ASP.NET Core MVC.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 49615c25d51cfa671157c2e56b8e0753719c678a
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710104"
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a>Praca z bazą danych LocalDB programu SQL Server w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

`MvcMovieContext` Obiektu obsługuje zadania z bazą danych i mapowania `Movie` obiekty do rekordów bazy danych. Kontekst bazy danych jest zarejestrowany w [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w `ConfigureServices` method in Class metoda *Startup.cs* pliku:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

::: moniker-end

ASP.NET Core [konfiguracji](xref:fundamentals/configuration/index) odczyty system `ConnectionString`. Dla wdrożenia lokalnego, pobiera parametry połączenia z *appsettings.json* pliku:

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

Podczas wdrażania aplikacji na serwerze testowym lub produkcyjnym, można użyć zmiennej środowiskowej lub innej metody, które można ustawić parametrów połączenia do rzeczywistego programu SQL Server. Zobacz [konfiguracji](xref:fundamentals/configuration/index) Aby uzyskać więcej informacji.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

LocalDB to Uproszczona wersja aparatu programu SQL Server Express bazy danych, która jest przeznaczona do tworzenia programu. LocalDB rozpoczyna się na żądanie i działa w trybie użytkownika, więc nie ma żadnych złożonej konfiguracji. Domyślnie tworzy bazy danych LocalDB "\*.mdf" pliki *C:/Users/\<użytkownika\>*  katalogu.

* Z **widoku** menu Otwórz **Eksplorator obiektów SQL Server** (SSOX).

  ![Menu Widok](working-with-sql/_static/ssox.png)

* Kliknij prawym przyciskiem myszy `Movie` tabeli **> Projektant widoków**

  ![Menu kontekstowe jest otwarte w tabeli filmu](working-with-sql/_static/design.png)

  ![Otwórz w Projektancie tabel filmu](working-with-sql/_static/dv.png)

Należy zauważyć ikonę klucza, obok `ID`. Domyślnie program EF spowoduje, że właściwość o nazwie `ID` klucz podstawowy.

* Kliknij prawym przyciskiem myszy `Movie` tabeli **> Dane widoku**

  ![Menu kontekstowe jest otwarte w tabeli filmu](working-with-sql/_static/ssox2.png)

  ![Tabela filmu otwarte, wyświetlanie tabeli danych](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a>Inicjowanie bazy danych

Utwórz nową klasę o nazwie `SeedData` w *modeli* folderu. Zastąp wygenerowany kod następujących czynności:

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

Czy istnieją wszystkie filmy w bazie danych, zwraca inicjatora inicjatora i filmy nie zostaną dodane.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Dodaj inicjator inicjatora

Zastąp zawartość *Program.cs* następującym kodem:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

Dodaj inicjator inicjator, tak aby `Main` method in Class metoda *Program.cs* pliku:

[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

::: moniker-end

Testowanie aplikacji

* Usuń wszystkie rekordy w bazie danych. Można to zrobić, wraz z łączami delete w przeglądarce lub SSOX.
* Wymusić na aplikacji, aby zainicjować (wywoływanie metody w `Startup` klasy), uruchamia seed — metoda. Aby wymusić inicjowania, usługi IIS Express musi zostać zatrzymana i uruchomiona ponownie. To zrobić przy użyciu dowolnej z następujących metod:

  * Kliknij prawym przyciskiem myszy ikonę na pasku zadań systemu usług IIS Express w obszarze powiadomień, a następnie naciśnij pozycję **zakończenia** lub **zatrzymanie witryny**

    ![Usługi IIS Express ikony na pasku zadań](working-with-sql/_static/iisExIcon.png)

    ![Menu kontekstowe](working-with-sql/_static/stopIIS.png)

    * Podczas uruchamiania w trybie bez debugowania programu VS, naciśnij klawisz F5, aby uruchomić tryb debugowania
    * W przypadku uruchomienia programu VS w trybie debugowania, Zatrzymaj debuger, a następnie naciśnij klawisz F5

Aplikacja zawiera wprowadzonych danych.

![Aplikacja filmu MVC otworzyć w programie Microsoft Edge wyświetlanie danych filmu](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> [Poprzednie](adding-model.md)
> [dalej](controller-methods-views.md)  
