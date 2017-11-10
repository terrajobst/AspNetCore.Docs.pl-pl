---
title: Dodawanie modelu dla aplikacji platformy ASP.NET Core MVC
author: rick-anderson
description: Dodawanie modelu do prostej aplikacji platformy ASP.NET Core.
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-00ee-4d66-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 7469546494ec54bfe36bc5bd2f5f9702889ddf4a
ms.sourcegitcommit: 2e61e287e220eddd5f3f4cd9147aa6417cfd9236
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/12/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

Uwaga: Szablony ASP.NET Core 2.0 zawierają *modele* folderu.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **MvcMovie** projektu > **Dodaj** > **nowy Folder**. Nazwa folderu *modele*.

Kliknij prawym przyciskiem myszy *modele* folder > **Dodaj** > **klasy**. Nazwa klasy **film** i dodaj następujące właściwości:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego. 

Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów. Masz teraz **M**odelu w Twojej **M**VC aplikacji.

## <a name="scaffolding-a-controller"></a>Tworzenia szkieletu kontrolera

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folderu **> Dodaj > kontrolera**.

![Widok powyżej kroku](adding-model/_static/add_controller.png)

W **Dodaj zależności MVC** okno dialogowe, wybierz opcję **minimalnego zależności**i wybierz **Dodaj**.

![Widok powyżej kroku](adding-model/_static/add_depend.png)

Visual Studio dodaje zależności niezbędne do tworzenia szkieletu z kontrolerem, ale sam kontroler nie został utworzony. Następny invoke z **> Dodaj > kontrolera** tworzy kontroler. 

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *kontrolerów* folderu **> Dodaj > kontrolera**.

![Widok powyżej kroku](adding-model/_static/add_controller.png)

W **Dodawanie szkieletu** okno dialogowe, naciśnij **kontroler MVC z widokami używający narzędzia Entity Framework > Dodaj**.

![Dodawanie szkieletu okna dialogowego](adding-model/_static/add_scaffold2.png)

Zakończenie **Dodaj kontroler** okna dialogowego:

* **Klasa modelu:** *Movie (MvcMovie.Models)*
* **Klasa kontekstu danych:** wybierz  **+**  ikonę i Dodaj domyślny **MvcMovie.Models.MvcMovieContext**

![Dodaj kontekst danych](adding-model/_static/dc.png)

* **Widoki:** zachować zaznaczone domyślne każdej z nich
* **Nazwa kontrolera:** Zachowaj ustawienie domyślne *MoviesController*
* Wybierz **Dodaj**

![Dodawanie kontrolera okna dialogowego](adding-model/_static/add_controller2.png)

Program Visual Studio tworzy:

* Entity Framework Core [bazy danych klasy kontekstu](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)
* Kontroler filmów (*Controllers/MoviesController.cs*)
* Pliki widoku razor dla stron Create, Delete, szczegóły, edycji i indeksu (*widoków/filmów/&ast;.cshtml*)

Automatyczne tworzenie kontekstu bazy danych i [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoki nosi nazwę *szkieletów*. Konieczne będzie wkrótce aplikacji funkcjonalnej sieci web, która umożliwia zarządzanie filmu bazy danych.

Uruchom aplikację i kliknięcie na **Mvc Movie** link, zostanie wyświetlony błąd podobny do następującego:

```
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

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![PMC menu](adding-model/_static/pmc.png)

W kryterium wprowadź następujące polecenia:

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**Uwaga:** Jeśli wystąpi błąd z `Install-Package` polecenia, otwórz Menedżera pakietów NuGet i wyszukaj `Microsoft.EntityFrameworkCore.Tools` pakietu. Dzięki temu można zainstalować pakiet lub sprawdź, czy jest on już zainstalowany. Zobacz też [podejście CLI](#cli) Jeśli masz problemy z kryterium.

`Add-Migration` Polecenie tworzy kod w celu utworzenia schematu początkowej bazy danych. Schemat jest oparta na modelu określone w `DbContext`(w *Data/MvcMovieContext.cs* pliku). `Initial` Argument jest używany do nazywania migracji. Można użyć dowolnej nazwy, ale Konwencja wybierz nazwę, która opisuje migracji. Zobacz [wprowadzenie do migracji](xref:data/ef-mvc/migrations#introduction-to-migrations) Aby uzyskać więcej informacji.

`Update-Database` Polecenia `Up` metody w *migracje /\<sygnatury czasowej > _InitialCreate.cs* pliku, który utworzy bazę danych.

<a name="cli"></a>Należy wykonać czynności poprzedzających przy użyciu interfejsu wiersza polecenia (CLI) zamiast PMC:

* Dodaj [EF podstawowych narzędzi](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) do *.csproj* pliku.
* Uruchom następujące polecenia z poziomu konsoli (w katalogu projektu):

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Menu kontekstowe IntelliSense w elemencie modelu listę dostępnych właściwości dla Identyfikatora, Price Data wydania i tytuł](adding-model/_static/ints.png)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Pomocników tagów](xref:mvc/views/tag-helpers/intro)
* [Lokalizacja i globalizacja](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Dodawanie widoku poprzedniej](adding-view.md)
[obok Praca z SQL](working-with-sql.md)  
