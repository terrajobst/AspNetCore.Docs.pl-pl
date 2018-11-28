---
title: Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i Visual Studio
author: rick-anderson
description: Tworzenie internetowego interfejsu API platformy ASP.NET Core MVC i programu Visual Studio w Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: eb23d5376742e04712f714263815582fddc5d18e
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450700"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a>Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i Visual Studio

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Mike Wasson](https://github.com/mikewasson)

Ten samouczek opiera się internetowego interfejsu API do zarządzania listę elementów "do wykonania". Nie jest tworzony interfejs użytkownika (UI).

Istnieją trzy wersje tego samouczka:

* Windows: Web API za pomocą programu Visual Studio w Windows (tego samouczka, zobacz [wersja wideo](https://www.youtube.com/watch?v=TTkhEyGBfAk))
* System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)
* macOS i Linux, Windows: [interfejsu API sieci Web za pomocą programu Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

> [!NOTE]
> W tej chwili testujemy użyteczność nowo zaproponowanej struktury spisu treści dla dokumentacji platformy ASP.NET Core.  Jeśli możesz poświęcić chwilę na wykonanie ćwiczenia polegającego na znalezieniu 7 różnych tematów w aktualnym lub zaproponowanym spisie treści, [kliknij tutaj i weź udział w badaniu](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a>Utwórz projekt

Wykonaj następujące kroki w programie Visual Studio:

* Z **pliku** menu, wybierz opcję **New** > **projektu**.
* Wybierz **aplikacji sieci Web programu ASP.NET Core** szablonu. Nadaj projektowi nazwę *TodoApi* i kliknij przycisk **OK**.
* W **nowej podstawowej aplikacji sieci Web ASP.NET - TodoApi** okno dialogowe, wybierz wersję platformy ASP.NET Core. Wybierz **API** szablon i kliknij przycisk **OK**. Czy **nie** wybierz **włączyć obsługę platformy Docker**.

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Program Visual Studio otworzy w przeglądarce i przechodzi do `http://localhost:<port>/api/values`, gdzie `<port>` jest numer portu wybranego losowo. Chrome, Microsoft Edge i przeglądarki Firefox wyświetlane następujące dane wyjściowe:

```json
["value1","value2"]
```

Jeśli używasz programu Internet Explorer, zostanie wyświetlony monit, aby zapisać *values.json* pliku.

### <a name="add-a-model-class"></a>Dodawanie klasy modelu

Model jest obiekt reprezentujący dane w aplikacji. W takim przypadku tylko model jest element do wykonania.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt. Wybierz **Dodaj** > **nowy Folder**. Nazwa folderu *modeli*.

> [!NOTE]
> Klasy modeli można przejść w dowolnym miejscu w projekcie. *Modeli* folder jest używany przez Konwencję dla klasy modelu.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**. Nazwa klasy *TodoItem* i kliknij przycisk **Dodaj**.

Aktualizacja `TodoItem` klasy z następującym kodem:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Baza danych generuje `Id` podczas `TodoItem` zostanie utworzony.

### <a name="create-the-database-context"></a>Utwórz kontekst bazy danych

*Kontekst bazy danych* jest główna klasa, która służy do koordynowania funkcje modelu danych Entity Framework. Ta klasa jest tworzona przez pochodząca od `Microsoft.EntityFrameworkCore.DbContext` klasy.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj** > **klasy**. Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.

Zastąp klasę z następującym kodem:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Dodawanie kontrolera

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *kontrolerów* folderu. Wybierz **Dodaj** > **nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasa formantu API** szablonu. Nazwa klasy *TodoController*i kliknij przycisk **Dodaj**.

![Dodaj okno dialogowe nowego elementu za pomocą kontrolera w wyszukiwania sieci web i pole Kontroler interfejsu API wybrane](first-web-api/_static/new_controller.png)

Zastąp klasę z następującym kodem:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Program Visual Studio otworzy w przeglądarce i przechodzi do `http://localhost:<port>/api/values`, gdzie `<port>` jest numer portu wybranego losowo. Przejdź do `Todo` kontroler na `http://localhost:<port>/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
