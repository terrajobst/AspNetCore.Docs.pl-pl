---
title: Dodawanie modelu do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dodawanie modelu do prostą aplikację platformy ASP.NET Core.
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 5a820789ee3a761025d09aa78f3c42e59fc5fa38
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011381"
---
[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

Kliknij prawym przyciskiem myszy *modeli* folder > **Dodaj** > **klasy**. Nazwa klasy **filmu** i dodaj następujące właściwości:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego. 

Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów. Masz teraz **M**odelu w swojej **M**VC aplikacji.

## <a name="scaffolding-a-controller"></a>Tworzenia szkieletów kontrolera

::: moniker range=">= aspnetcore-2.1"

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folderu **> Dodaj > Nowy element szkieletu**.

![Widok powyżej kroku](adding-model/_static/add_controller21.png)

W **Dodawanie szkieletu** okno dialogowe, naciśnij **kontroler MVC z widokami używający narzędzia Entity Framework > Dodaj**.

![Dodaj okno dialogowe Tworzenie szkieletu](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folderu **> Dodaj > kontrolera**.

![Widok powyżej kroku](adding-model/_static/add_controller.png)

Jeśli **Dodaj zależności MVC** zostanie wyświetlone okno dialogowe:

* [Aktualizacja programu Visual Studio do najnowszej wersji](https://www.visualstudio.com/downloads/). Wersje serwera Visual Studio przed 15.5 pokazuj tego okna dialogowego.
* Jeśli nie można zaktualizować wybierz **Dodaj**, a następnie ponownie wykonaj kroki kontrolera Dodaj.

W **Dodawanie szkieletu** okno dialogowe, naciśnij **kontroler MVC z widokami używający narzędzia Entity Framework > Dodaj**.

![Dodaj okno dialogowe Tworzenie szkieletu](adding-model/_static/add_scaffold2.png)

::: moniker-end

Wykonaj **Dodaj kontroler** okno dialogowe:

* **Klasa modelu:** *Movie (MvcMovie.Models)*
* **Klasa kontekstu danych:** wybierz **+** ikonę i Dodaj domyślny **MvcMovie.Models.MvcMovieContext**

![Dodawanie kontekstu danych](adding-model/_static/dc.png)

* **Widoki:** Zachowaj domyślne poszczególnych opcji zaznaczone
* **Nazwa kontrolera:** Zachowaj ustawienie domyślne *MoviesController*
* Naciśnij pozycję **Dodaj**

![Dodaj kontroler, okno dialogowe](adding-model/_static/add_controller2.png)

Program Visual Studio tworzy:

* Entity Framework Core [bazy danych klasy kontekstu](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)
* Kontroler filmy (*Controllers/MoviesController.cs*)
* Pliki widoku razor dla stron Create, Delete, szczegółowe informacje, edycji i indeksu (<em>widoków/filmy/&ast;.cshtml</em>)

Automatyczne tworzenie kontekst bazy danych i [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoki są określane jako *tworzenia szkieletów*. Wkrótce będziesz mieć aplikację internetową w pełni funkcjonalne, która umożliwia zarządzanie filmu bazy danych.

Jeśli Uruchom aplikację i kliknąć **filmu Mvc** link, zostanie wyświetlony komunikat o błędzie podobny do następującego:

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

Musisz utworzyć bazę danych i użyjesz programu EF Core [migracje](xref:data/ef-mvc/migrations) funkcję, aby to zrobić. Migracje umożliwia tworzenie bazy danych, która pasuje do modelu danych i zaktualizować schemat bazy danych, gdy model danych, zmiany.

## <a name="add-ef-tooling-and-perform-initial-migration"></a>Dodaj EF narzędzi i wykonywania początkowej migracji

W tej sekcji użyjesz konsoli Menedżera pakietów (PMC) do:

* Dodaj pakiet narzędzi Entity Framework Core Tools. Ten pakiet jest wymagany do dodawania migracje i aktualizują bazę danych.
* Dodaj początkowej migracji.
* Zaktualizuj bazy danych przy użyciu początkowej migracji.

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet > Konsola Menedżera pakietów**.

<!-- following image shared with uid: tutorials/razor-pages/model --> ![Menu konsoli zarządzania Pakietami](adding-model/_static/pmc.png)

W konsoli zarządzania Pakietami wprowadź następujące polecenia:

::: moniker range=">= aspnetcore-2.1"

``` PMC
Add-Migration Initial
Update-Database
```

Ignoruj następujący komunikat o błędzie, możemy naprawić w następnym samouczku:

*Microsoft.EntityFrameworkCore.Model.Validation[30000]*  
      *Brak typu została określona dla dziesiętną kolumny "Cena" jednostki typu "Filmu". To spowoduje, że wartości, aby dyskretnie obcięty, jeśli nie mieszczą się w domyślnej dokładności i skali. Jawnie określić typ kolumny serwera SQL, która może pomieścić wszystkie wartości przy użyciu "ForHasColumnType()".*

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**Uwaga:** Jeśli otrzymasz komunikat o błędzie z `Install-Package` polecenia Otwórz Menedżera pakietów NuGet i wyszukiwania oraz `Microsoft.EntityFrameworkCore.Tools` pakietu. Dzięki temu można zainstalować pakietu, lub sprawdź, czy jest on już zainstalowany. Możesz również zapoznać się [podejście interfejsu wiersza polecenia](#cli) Jeśli masz problemy z konsoli zarządzania Pakietami.

::: moniker-end

`Add-Migration` Polecenie tworzy kod, aby utworzyć schemat początkowej bazy danych. Schemat jest oparta na modelu, określone w `DbContext`(w *Data/MvcMovieContext.cs* pliku). `Initial` Argument jest używany do nazywania migracje. Można użyć dowolnej nazwy, ale zgodnie z Konwencją wybierz nazwę, która opisuje migracji. Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.

`Update-Database` Polecenia `Up` method in Class metoda *migracje /\<sygnatura czasowa > _Initial.cs* pliku, który tworzy bazę danych.

<a name="cli"></a> Zamiast konsoli zarządzania Pakietami, należy wykonać czynności poprzedzających przy użyciu interfejsu wiersza polecenia (CLI):

* Dodaj [narzędzi programu EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) do *.csproj* pliku.
* Uruchom następujące polecenia z poziomu konsoli (w katalogu projektu):

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  Jeśli uruchamianie aplikacji, a komunikat o błędzie:

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

![Menu kontekstowe funkcji IntelliSense dla elementu modelu, wyświetlanie listy dostępnych właściwości dla Identyfikatora, ceny, Data wydania i tytuł](adding-model/_static/ints.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Pomocnicy tagów](xref:mvc/views/tag-helpers/intro)
* [Globalizacja i lokalizacja](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Poprzednie, Dodawanie widoku](adding-view.md)
> [obok pracy przy użyciu języka SQL](working-with-sql.md)  
