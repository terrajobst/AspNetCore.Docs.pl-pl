---
title: Dodawanie modelu dla aplikacji ASP.NET Core MVC.
author: rick-anderson
description: Dodawanie modelu do prostej aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 09/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 81511b05a3cc11a58b93452d3c6e5305e7ee4357
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* Dodaj klasę do *modele* folder o nazwie *Movie.cs*.
* Dodaj następujący kod do *Models/Movie.cs* pliku:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego. 

Utworzyć aplikację w celu zweryfikowania nie zawiera błędów i na koniec dodano **M**odelu do Twojej **M**VC aplikacji.

## <a name="prepare-the-project-for-scaffolding"></a>Przygotowanie projektu do tworzenia szkieletów

- Dodaj następujące wyróżnione pakietów NuGet do *MvcMovie.csproj* pliku:
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- Zapisz plik i wybierz **przywrócić** do **informacji** komunikatu "Istnieją nierozwiązane zależności".
- Utwórz *Models/MvcMovieContext.cs* pliku i dodaj następującą `MvcMovieContext` klasy:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- Otwórz *Startup.cs* plik i dodać dwie deklaracje Using:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- Kontekst bazy danych, aby dodać *Startup.cs* pliku:

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  Ta wartość informuje klas modelu, które znajdują się w modelu danych programu Entity Framework. Definiujesz co *zestaw jednostek* filmu obiektów, które będą reprezentowane w bazie danych jako tabelę filmu.

- Skompiluj projekt, aby sprawdzić, czy nie ma żadnych błędów.

## <a name="scaffold-the-moviecontroller"></a>Tworzenie szkieletu MovieController

Otwórz okno terminala w folderze projektu i uruchom następujące polecenia:

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
Aparat szkieletów tworzy następujące czynności:

* Kontroler filmów (*Controllers/MoviesController.cs*)
* Pliki widoku razor dla stron Create, Delete, szczegóły, edycji i indeksu (*widoków/filmów/\*.cshtml*)

Automatyczne tworzenie [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoki nosi nazwę *szkieletów*. Konieczne będzie wkrótce aplikacji funkcjonalnej sieci web, która umożliwia zarządzanie filmu bazy danych.

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

Masz teraz bazę danych i stron do wyświetlania, edytowania, aktualizacji i usuwania danych. W następnym samouczku firma Microsoft będzie współpracować z bazy danych.

### <a name="additional-resources"></a>Dodatkowe zasoby

* [Pomocnicy tagów](xref:mvc/views/tag-helpers/intro)
* [Globalizacja i lokalizacja](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Poprzednie — Dodawanie widoku](adding-view.md)
[następne — Praca z bazy danych SQLite](working-with-sql.md)
