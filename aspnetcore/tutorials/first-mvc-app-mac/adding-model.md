---
title: Dodaj model do aplikacji platformy ASP.NET Core MVC
author: rick-anderson
description: Dodawanie modelu do prostej aplikacji platformy ASP.NET Core.
ms.author: riande
manager: wpickett
ms.devlang: csharp
ms.date: 09/22/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: .net-core
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 3a7db5e435fe72150feb1e0ec905b6f6adc16f2c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* Kliknij prawym przyciskiem myszy *modele* folder, a następnie wybierz **Dodaj** > **nowy plik**. 
* W **nowy plik** okna dialogowego:

  * Wybierz **ogólne** w okienku po lewej stronie.
  * Wybierz **pustą klasę** w słabe center.
  * Nazwa klasy **film** i wybierz **nowy**.

Dodaj następujące właściwości `Movie` klasy:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

`ID` Pole jest wymagane przez bazę danych dla klucza podstawowego.

Skompiluj projekt, aby sprawdzić, czy nie zawiera błędów. Masz teraz **M**odelu w Twojej **M**VC aplikacji.

## <a name="prepare-the-project-for-scaffolding"></a>Przygotowanie projektu do tworzenia szkieletów

- Kliknij prawym przyciskiem myszy plik projektu, a następnie wybierz **Narzędzia > Edytuj plik**.

  ![Widok powyżej kroku](adding-model/_static/1.png)

- Dodaj następujące wyróżnione pakietów NuGet do *MvcMovie.csproj* pliku:
             
  [!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- Zapisz plik.

- Utwórz *Models/MvcMovieContext.cs* pliku i dodaj następującą `MvcMovieContext` klasy:[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- Otwórz *Startup.cs* plik i dodać dwie deklaracje Using:[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- Kontekst bazy danych, aby dodać *Startup.cs* pliku:

   [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  Ta wartość informuje klas modelu, które znajdują się w modelu danych programu Entity Framework. Definiujesz co *zestaw jednostek* filmu obiektów, które będą reprezentowane w bazie danych jako tabelę filmu.

- Skompiluj projekt, aby sprawdzić, czy nie ma żadnych błędów.

## <a name="scaffold-the-moviecontroller"></a>Tworzenie szkieletu MovieController

Otwórz okno terminala w folderze projektu i uruchom następujące polecenia:

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
Jeśli zostanie wyświetlony błąd `No executable found matching command "dotnet-aspnet-codegenerator", verify`:

 * Znajdujesz się w katalogu projektu. Katalog projektu ma *Program.cs*, *Startup.cs* i *.csproj* plików.
 * Używana wersja dotnet jest 1.1 lub nowszej. Uruchom `dotnet` Aby pobrać wersję.
 * Dodano `<DotNetCliToolReference>` elementu [pliku MvcMovie.csproj](#prepare-the-project-for-scaffolding).
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

Aparat szkieletów tworzy następujące czynności:

* Kontroler filmów (*Controllers/MoviesController.cs*)
* Pliki widoku razor dla stron Create, Delete, szczegóły, edycji i indeksu (*widoków/filmów/\*.cshtml*)

Automatyczne tworzenie [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (tworzenia, odczytu, aktualizacji i usuwania) metody akcji i widoki nosi nazwę *szkieletów*. Konieczne będzie wkrótce aplikacji funkcjonalnej sieci web, która umożliwia zarządzanie filmu bazy danych.

### <a name="add-the-files-to-visual-studio"></a>Dodaj pliki do programu Visual Studio

* Dodaj *MovieController.cs* plik do projektu programu Visual Studio:

  * Kliknij prawym przyciskiem myszy *kontrolerów* i wybierz polecenie **Dodaj > Dodaj pliki**.
  * Wybierz *MovieController.cs* pliku.

* Dodaj *filmy* folderów i widoków:

  * Kliknij prawym przyciskiem myszy *widoków* i wybierz polecenie **Dodaj > Dodaj istniejący Folder**.
  * Przejdź do *widoków* folderu, wybierz opcję *Views\Movies*, a następnie wybierz **Otwórz**.
  * W **wybierz pliki, aby dodać z filmów** zaznacz pozycję **obejmują wszystkie**, a następnie **OK**.

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

Masz teraz bazę danych i stron do wyświetlania, edytowania, aktualizacji i usuwania danych. W następnym samouczku firma Microsoft będzie współpracować z bazy danych.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Pomocnicy tagów](xref:mvc/views/tag-helpers/intro)
* [Globalizacja i lokalizacja](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Dodawanie widoku poprzedniej](adding-view.md)
[obok Praca z SQL](working-with-sql.md)  
