---
title: Tworzenie interfejsu API sieci Web przy użyciu programu Visual Studio i ASP.NET Core dla systemu Windows
author: rick-anderson
description: Tworzenie składnika web API platformy ASP.NET Core MVC i Visual Studio dla systemu Windows
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 865f4345f2d135375b7ed7c3d5199bdbaa1aae50
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/08/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>Tworzenie interfejsu API sieci Web przy użyciu programu Visual Studio i ASP.NET Core dla systemu Windows

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)

W tym samouczku tworzy interfejs API sieci web do zarządzania listę elementów "do wykonania". Interfejs użytkownika (UI) nie jest tworzone.

Istnieją trzy wersje tego samouczka:

* Systemu Windows: Interfejs API z programu Visual Studio dla systemu Windows (w tym samouczku) sieci Web
* System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)
* System macOS, Linux, Windows: [interfejsu API sieci Web z kodem Visual Studio](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a>Utwórz projekt

Wykonaj następujące czynności w programie Visual Studio:

* Z **pliku** menu, wybierz opcję **nowy** > **projektu**.
* Wybierz **aplikacji sieci Web platformy ASP.NET Core** szablonu. Nazwij projekt *TodoApi* i kliknij przycisk **OK**.
* W **nowe podstawowe aplikacji sieci Web ASP.NET - TodoApi** okno dialogowe, wybierz wersję platformy ASP.NET Core. Wybierz **interfejsu API** szablon i kliknij przycisk **OK**. Czy **nie** wybierz **Włącz obsługę Docker**.

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio naciśnij kombinację klawiszy CTRL + F5, aby uruchomić aplikację. Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:<port>/api/values`, gdzie `<port>` jest liczbą losowo wybranego portu. Chrome, Microsoft Edge i przeglądarki Firefox można wyświetlić następujące dane wyjściowe:

```json
["value1","value2"]
```

### <a name="add-a-model-class"></a>Dodaj klasę modelu

Model jest obiekt reprezentujący danych w aplikacji. W takim przypadku tylko model jest zadanie do wykonania.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt. Wybierz **dodać** > **nowy Folder**. Nazwa folderu *modele*.

> [!NOTE]
> Klasy modelu może przejść w dowolnym miejscu w projekcie. *Modele* folder jest używany przez Konwencję dla klas modelu.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *modele* i wybierz polecenie **Dodaj** > **klasy**. Nazwa klasy *TodoItem* i kliknij przycisk **Dodaj**.

Aktualizacja `TodoItem` klasy następującym kodem:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.

### <a name="create-the-database-context"></a>Tworzenie kontekstu bazy danych

*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych. Ta klasa jest tworzona przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *modele* i wybierz polecenie **Dodaj** > **klasy**. Nazwa klasy *TodoContext* i kliknij przycisk **Dodaj**.

Zastąp klasę z następującym kodem:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE [Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Dodawanie kontrolera

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *kontrolerów* folderu. Wybierz **dodać** > **nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz opcję **Klasa kontrolera interfejsu API** szablonu. Nazwa klasy *TodoController*i kliknij przycisk **Dodaj**.

![Okno dialogowe nowego elementu z kontrolerem dodatek do wyszukiwania sieci web i pole Kontroler interfejsu API wybrane](first-web-api/_static/new_controller.png)

Zastąp klasę z następującym kodem:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio naciśnij kombinację klawiszy CTRL + F5, aby uruchomić aplikację. Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:<port>/api/values`, gdzie `<port>` jest liczbą losowo wybranego portu. Przejdź do `Todo` kontroler na `http://localhost:<port>/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
