---
title: Dodawanie modelu dla aplikacji ASP.NET Core MVC.
author: rick-anderson
description: Dodawanie modelu do prostej aplikacji platformy ASP.NET Core.
ms.author: riande
ms.date: 09/18/2017
ms.topic: get-started-article
ms.technology: aspnet
keywords: "Platformy ASP.NET Core, WebAPI, interfejs API sieci Web, REST, Mac, Linux, HTTP, usługi, usługi HTTP kodzie VS"
ms.prod: asp.net-core
manager: wpickett
ms.assetid: 8dc28498-eeee-4666-b903-b593059e9f39
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 70aa344ca4ceafacf53907c925fd595e47104d7e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
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

> [!NOTE]
> Jeśli wystąpi błąd podczas wykonywania polecenia funkcja szkieletów, zobacz [wystawiać 444 w repozytorium szkieletów](https://github.com/aspnet/scaffolding/issues/444) obejście tego problemu.

Aparat szkieletów tworzy następujące czynności:

* Kontroler filmów (*Controllers/MoviesController.cs*)
* Pliki widoku razor dla stron Create, Delete, szczegóły, edycji i indeksu (*widoków/filmów/\*.cshtml*)

Automatyczne tworzenie [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoki nosi nazwę *szkieletów*. Konieczne będzie wkrótce aplikacji funkcjonalnej sieci web, która umożliwia zarządzanie filmu bazy danych.

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

Masz teraz bazę danych i stron do wyświetlania, edytowania, aktualizacji i usuwania danych. W następnym samouczku firma Microsoft będzie współpracować z bazy danych.

### <a name="additional-resources"></a>Dodatkowe zasoby

* [Pomocników tagów](xref:mvc/views/tag-helpers/intro)
* [Lokalizacja i globalizacja](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Poprzednie — Dodawanie widoku](adding-view.md)
[następne — Praca z bazy danych SQLite](working-with-sql.md)
