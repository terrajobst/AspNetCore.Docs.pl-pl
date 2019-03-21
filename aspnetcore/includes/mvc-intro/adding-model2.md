---
ms.openlocfilehash: de093954fedc0fef1f945e881e1c7a6a24178bdb
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265410"
---
## <a name="add-initial-migration-and-update-the-database"></a>Dodawanie początkowej migracji i aktualizowanie bazy danych

* Otwórz wiersz polecenia i przejdź do katalogu projektu. (Katalogu zawierającego *Startup.cs* pliku).

* W wierszu polecenia wpisz następujące polecenia:

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  [.NET core](/dotnet/core/tools/index) jest implementacją dla wielu platform .NET. Oto, wykonaj następujące polecenia:

  * [DotNet restore](/dotnet/core/tools/dotnet-restore): Pobiera pakiety NuGet, określone w *.csproj* pliku.
  * `dotnet ef migrations add Initial` Uruchamia polecenie migracje Entity Framework .NET Core interfejsu wiersza polecenia i tworzy początkowej migracji. Parametr po "add" to nazwa, który przypiszesz do migracji. W tym miejscu możesz teraz nazewnictwa migracji "Początkowego" ponieważ jest on początkowej migracji bazy danych. Ta operacja tworzy *danych/Migrations/\<daty / godziny > _Initial.cs* plik zawierający polecenia migracji, aby dodać *filmu* tabeli w bazie danych.
  * `dotnet ef database update`  Aktualizuje bazę danych do migracji, którą właśnie utworzyliśmy.

W następnym samouczku dowiesz się o bazy danych i parametry połączenia. Dowiesz się o zmianach w modelu danych [dodać pole](xref:tutorials/first-mvc-app/new-field) samouczka.
