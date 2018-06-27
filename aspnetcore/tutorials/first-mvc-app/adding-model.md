---
title: Dodaj model do aplikacji platformy ASP.NET Core MVC
author: rick-anderson
description: Dodawanie modelu do prostej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 28a63498bc1a3c7b6ad6be038209dacdb49e44ee
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960671"
---
[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

Kliknij prawym przyciskiem myszy *modele* folder > **Dodaj** > **klasy**. Nazwa klasy **film** i dodaj następujące właściwości:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego. 

Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów. Masz teraz **M**odelu w Twojej **M**VC aplikacji.

## <a name="scaffolding-a-controller"></a>Tworzenia szkieletu kontrolera

::: moniker range=">= aspnetcore-2.1"

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folderu **> Dodaj > Nowy element szkieletu**.

![Widok powyżej kroku](adding-model/_static/add_controller21.png)

W **Dodawanie szkieletu** okno dialogowe, naciśnij **kontroler MVC z widokami używający narzędzia Entity Framework > Dodaj**.

![Dodawanie szkieletu okna dialogowego](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folderu **> Dodaj > kontrolera**.

![Widok powyżej kroku](adding-model/_static/add_controller.png)

Jeśli **Dodaj zależności MVC** zostanie wyświetlone okno dialogowe:

* [Aktualizowanie do najnowszej wersji programu Visual Studio](https://www.visualstudio.com/downloads/). Visual Studio wersje poprzedzające 15,5 cala pokazuj tego okna dialogowego.
* Jeśli nie można zaktualizować, wybierz **Dodaj**, a następnie ponownie wykonaj kroki kontrolera Dodaj.

W **Dodawanie szkieletu** okno dialogowe, naciśnij **kontroler MVC z widokami używający narzędzia Entity Framework > Dodaj**.

![Dodawanie szkieletu okna dialogowego](adding-model/_static/add_scaffold2.png)

::: moniker-end

Zakończenie **Dodaj kontroler** okna dialogowego:

* **Klasa modelu:** *Movie (MvcMovie.Models)*
* **Klasa kontekstu danych:** wybierz **+** ikonę i Dodaj domyślny **MvcMovie.Models.MvcMovieContext**

![Dodaj kontekst danych](adding-model/_static/dc.png)

* **Widoki:** zachować zaznaczone domyślne każdej z nich
* **Nazwa kontrolera:** Zachowaj ustawienie domyślne *MoviesController*
* Wybierz **Dodaj**

![Dodawanie kontrolera okna dialogowego](adding-model/_static/add_controller2.png)

Program Visual Studio tworzy:

* Entity Framework Core [bazy danych klasy kontekstu](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)
* Kontroler filmów (*Controllers/MoviesController.cs*)
* Pliki widoku razor dla stron Create, Delete, szczegóły, edycji i indeksu (<em>widoków/filmów/&ast;.cshtml</em>)

Automatyczne tworzenie kontekstu bazy danych i [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoki nosi nazwę *szkieletów*. Konieczne będzie wkrótce aplikacji funkcjonalnej sieci web, która umożliwia zarządzanie filmu bazy danych.

Uruchom aplikację i kliknięcie na **Mvc Movie** łącza, wystąpi błąd podobny do następującego:

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

Należy utworzyć bazę danych i użyjesz EF Core [migracje](xref:data/ef-mvc/migrations) funkcji w tym celu. Migracje umożliwia tworzenie bazy danych, która jest zgodna z modelem danych i zaktualizować schemat bazy danych, gdy model danych, zmiany.

## <a name="add-ef-tooling-and-perform-initial-migration"></a>Dodaj EF narzędzi i przeprowadzić migrację początkowej

W tej sekcji służą do konsoli Menedżera pakietów (PMC):

* Dodaj pakiet Entity Framework podstawowe narzędzia. Ten pakiet jest wymagany do dodawania migracji i aktualizacji bazy danych.
* Dodaj początkowej migracji.
* Aktualizacji bazy danych z początkowej migracji.

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > konsoli Menedżera pakietów**.

<!-- following image shared with uid: tutorials/razor-pages/model --> ![PMC menu](adding-model/_static/pmc.png)

W kryterium wprowadź następujące polecenia:

::: moniker range=">= aspnetcore-2.1"
``` PMC
Add-Migration Initial
Update-Database
```

Ignoruj następujący komunikat o błędzie, możemy naprawić w następnym samouczku:

*Microsoft.EntityFrameworkCore.Model.Validation[30000]*  
      *Typ nie został określony dla kolumny dziesiętną "Cena" na typ jednostki "Filmu". Spowoduje to wartości, aby dyskretnie obcięty, jeśli nie mieszczą się w domyślnej precyzję i skalę. Jawnie określić typ kolumny serwera SQL, która może obsłużyć wszystkie wartości przy użyciu "ForHasColumnType()".*

::: moniker-end
::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**Uwaga:** Jeśli wystąpi błąd z `Install-Package` polecenia, otwórz Menedżera pakietów NuGet i wyszukaj `Microsoft.EntityFrameworkCore.Tools` pakietu. Dzięki temu można zainstalować pakiet lub sprawdź, czy jest on już zainstalowany. Zobacz też [podejście CLI](#cli) Jeśli masz problemy z kryterium.

::: moniker-end

`Add-Migration` Polecenie tworzy kod w celu utworzenia schematu początkowej bazy danych. Schemat jest oparta na modelu określone w `DbContext`(w *Data/MvcMovieContext.cs* pliku). `Initial` Argument jest używany do nazywania migracji. Można użyć dowolnej nazwy, ale Konwencja wybierz nazwę, która opisuje migracji. Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.

`Update-Database` Polecenia `Up` metody w *migracje /\<sygnatury czasowej > _Initial.cs* pliku, który utworzy bazę danych.

<a name="cli"></a> Należy wykonać czynności poprzedzających przy użyciu interfejsu wiersza polecenia (CLI) zamiast PMC:

* Dodaj [EF podstawowych narzędzi](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) do *.csproj* pliku.
* Uruchom następujące polecenia z poziomu konsoli (w katalogu projektu):

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  Jeżeli możesz uruchomić aplikację i komunikat o błędzie:

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

Prawdopodobnie nie uruchomiono `dotnet ef database update`.

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]
::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Menu kontekstowe IntelliSense w elemencie modelu listę dostępnych właściwości dla Identyfikatora, Price Data wydania i tytuł](adding-model/_static/ints.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Pomocnicy tagów](xref:mvc/views/tag-helpers/intro)
* [Globalizacja i lokalizacja](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Dodawanie widoku poprzedniej](adding-view.md)
> [obok Praca z SQL](working-with-sql.md)  
