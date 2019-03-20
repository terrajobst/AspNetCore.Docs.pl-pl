---
ms.openlocfilehash: 86cf1874677dc8b79e3223fb0819eb1881c69a11
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265597"
---
<a name="dc"></a>

### <a name="add-a-database-context-class"></a>Dodawanie klasy kontekstu bazy danych

Dodaj następujący kod `RazorPagesMovieContext` klasy *modeli* folderu:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

Powyższy kod tworzy `DbSet` właściwość zestawu jednostek. W terminologii programu Entity Framework zwykle zestaw jednostek odnosi się do tabeli bazy danych, a jednostka odnosi się do wiersza w tabeli.

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a>Dodaj parametry połączenia bazy danych

Dodaj parametry połączenia, aby *appsettings.json* pliku:

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a>Dodawanie wymaganych pakietów NuGet

Uruchom następujące polecenie interfejsu wiersza polecenia platformy .NET Core, aby dodać bazy danych SQLite i CodeGeneration.Design do projektu:

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design

```

`Microsoft.VisualStudio.Web.CodeGeneration.Design` Pakietu jest wymagany do tworzenia szkieletów.

<a name="reg"></a>

### <a name="register-the-database-context"></a>Zarejestruj kontekst bazy danych

Dodaj następujący kod `using` instrukcji na górze *Startup.cs*:

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Zarejestruj kontekst bazy danych za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera w `Startup.ConfigureServices`.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

Skompiluj projekt, w celu sprawdzenia błędów.
