---
title: "Tworzenie interfejsu API sieci Web przy użyciu programu Visual Studio i ASP.NET Core dla systemu Windows"
author: rick-anderson
description: "Tworzenie składnika web API platformy ASP.NET Core MVC i Visual Studio dla systemu Windows"
keywords: "Platformy ASP.NET Core, WebAPI, interfejs API sieci Web, REST, HTTP, usługi, usługa HTTP"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 617b11cd7652e393c06446c62138802e4a4e90df
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/19/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>Tworzenie składnika web API platformy ASP.NET Core i Visual Studio dla systemu Windows

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Wasson Jan](https://github.com/mikewasson)

W tym samouczku będziesz kompilacji interfejsu API sieci web do zarządzania listę elementów "do wykonania". Nie Konstruuj interfejsu użytkownika.

Istnieją 3 wersje tego samouczka:

* Systemu Windows: Interfejs API z programu Visual Studio dla systemu Windows (w tym samouczku) sieci Web
* System macOS: [interfejsu API sieci Web z programem Visual Studio dla komputerów Mac](xref:tutorials/first-web-api-mac)
* System macOS, Linux, Windows: [interfejsu API sieci Web z kodem Visual Studio](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE[install 2.0](../includes/install2.0.md)]

Zobacz [ten plik PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) dla wersji platformy ASP.NET Core 1.1.

## <a name="create-the-project"></a>Utwórz projekt

W programie Visual Studio, wybierz **pliku** menu > **nowy** > **projektu**.

Wybierz **aplikacji sieci Web platformy ASP.NET Core (.NET Core)** szablonu projektu. Nazwij projekt `TodoApi` i wybierz **OK**.

![Okno dialogowe nowego projektu](first-web-api/_static/new-project.png)

W **nowe podstawowe aplikacji sieci Web ASP.NET - TodoApi** okno dialogowe, wybierz opcję **interfejsu API sieci Web** szablonu. Wybierz **OK**. Czy **nie** wybierz **Włącz obsługę Docker**.

![Okno dialogowe nowego aplikacji sieci Web platformy ASP.NET z szablonem projektu interfejsu API sieci Web z platformy ASP.NET Core szablonów](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio naciśnij kombinację klawiszy CTRL + F5, aby uruchomić aplikację. Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port/api/values`, gdzie *portu* jest numeru portu losowo wybrany. Chrome, Edge i przeglądarki Firefox wyświetlane są następujące:

```
["value1","value2"]
``` 

### <a name="add-a-model-class"></a>Dodaj klasę modelu

Model jest obiekt, który reprezentuje dane w aplikacji. W takim przypadku tylko model jest zadanie do wykonania.

Dodaj folder o nazwie "Modele". W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt. Wybierz **dodać** > **nowy Folder**. Nazwa folderu *modele*.

Uwaga: Klasy modelu Przejdź dowolne miejsce w projekcie, ale *modele* folder jest używany przez Konwencję.

Dodaj `TodoItem` klasy. Kliknij prawym przyciskiem myszy *modele* i wybierz polecenie **Dodaj** > **klasy**. Nazwa klasy `TodoItem` i wybierz **Dodaj**.

Zastąp wygenerowany kod poniżej:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

Generuje bazy danych `Id` podczas `TodoItem` jest tworzony.

### <a name="create-the-database-context"></a>Tworzenie kontekstu bazy danych

*Kontekst bazy danych* jest główna klasa, która koordynuje funkcji programu Entity Framework o dany model danych. Ta klasa jest tworzona przez pochodny `Microsoft.EntityFrameworkCore.DbContext` klasy.

Dodaj `TodoContext` klasy. Kliknij prawym przyciskiem myszy *modele* i wybierz polecenie **Dodaj** > **klasy**. Nazwa klasy `TodoContext` i wybierz **Dodaj**.

Zastąp wygenerowany kod poniżej:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Dodawanie kontrolera

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy *kontrolerów* folderu. Wybierz **dodać** > **nowy element**. W **Dodaj nowy element** okno dialogowe, wybierz opcję **klasy kontrolera interfejsu API sieci Web** szablonu. Nazwa klasy `TodoController`.

![Okno dialogowe nowego elementu z kontrolerem dodatek do wyszukiwania sieci web i pole Kontroler interfejsu API wybrane](first-web-api/_static/new_controller.png)

Zastąp wygenerowany kod poniżej:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]
  
### <a name="launch-the-app"></a>Uruchom aplikację

W programie Visual Studio naciśnij kombinację klawiszy CTRL + F5, aby uruchomić aplikację. Program Visual Studio spowoduje uruchomienie przeglądarki i przechodzi do `http://localhost:port/api/values`, gdzie *portu* jest liczbą losowo wybranego portu. Jeśli używasz programu Chrome, krawędzi lub Firefox, będą wyświetlane dane. Jeśli używasz programu Internet Explorer programu Internet Explorer będzie Monituj o po otwarciu lub zapisać *values.json* pliku. Przejdź do `Todo` kontrolera właśnie utworzyliśmy `http://localhost:port/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

