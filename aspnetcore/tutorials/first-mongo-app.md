---
title: Tworzenie interfejsów API sieci web za pomocą platformy ASP.NET Core i bazy danych MongoDB
author: prkhandelwal
description: W tym samouczku przedstawiono sposób tworzenia sieci web platformy ASP.NET Core interfejsu API przy użyciu bazy danych NoSQL bazy danych MongoDB.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 01/31/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: f593a8d2d06897736b12f49f25c6049ea994a88a
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610605"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a>Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i usługi MongoDB

Przez [Pratik Khandelwal](https://twitter.com/K2Prk) i [Scott Addie](https://twitter.com/Scott_Addie)

Ten samouczek tworzy internetowego interfejsu API, która wykonuje operacje tworzenia, odczytu, aktualizowania lub usuwania (CRUD) [bazy danych MongoDB](https://www.mongodb.com/what-is-mongodb) bazy danych NoSQL.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie bazy danych MongoDB
> * Tworzenie bazy danych MongoDB
> * Definiowanie kolekcji usługi MongoDB i schematu
> * Wykonywanie operacji CRUD bazy danych MongoDB z internetowego interfejsu API

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET core SDK 2,2 lub nowszy](https://www.microsoft.com/net/download/all)
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET core SDK 2,2 lub nowszy](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [Środowisko C# dla programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [MongoDB](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* [.NET core SDK 2,2 lub nowszy](https://www.microsoft.com/net/download/all)
* [Program Visual Studio dla komputerów Mac w wersji 7,7 lub nowszy](https://visualstudio.microsoft.com/downloads/)
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a>Konfigurowanie bazy danych MongoDB

Jeśli przy użyciu Windows, bazy danych MongoDB jest zainstalowana w *C:\\Program Files\\bazy danych MongoDB* domyślnie. Dodaj *C:\\Program Files\\bazy danych MongoDB\\serwera\\\<numer_wersji >\\bin* do `Path` zmiennej środowiskowej. Dzięki tej zmianie bazy danych MongoDB dostęp z dowolnego miejsca na komputerze deweloperskim.

Użyj powłoki mongo w poniższych krokach umożliwia tworzenie bazy danych, wprowadzanie kolekcje i przechowywanie dokumentów. Aby uzyskać więcej informacji na temat poleceń powłoki mongo, zobacz [Praca z powłoki mongo](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).

1. Wybierz katalog na komputerze deweloperskim do przechowywania danych. Na przykład *C:\\BooksData* na Windows. Utwórz katalog, jeśli nie istnieje. Powłoki mongo nie tworzyć nowe katalogi.
1. Otwórz powłokę wiersza polecenia. Uruchom następujące polecenie, aby nawiązać połączenie z bazą danych MongoDB na domyślnym porcie 27017. Pamiętaj, aby zastąpić `<data_directory_path>` z katalogu, który został wybrany w poprzednim kroku.

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. Otwórz inne wystąpienie powłoki poleceń. Połączenia z bazą danych testu domyślną, uruchamiając następujące polecenie:

    ```console
    mongo
    ```

1. Uruchom następujące polecenie w powłoce poleceń:

    ```console
    use BookstoreDb
    ```

    Jeśli jeszcze nie istnieje, bazę danych o nazwie *BookstoreDb* zostanie utworzony. Jeśli baza danych istnieje, jego połączenie jest otwarte dla transakcji.

1. Utwórz `Books` kolekcji za pomocą następującego polecenia:

    ```console
    db.createCollection('Books')
    ```

    Wyświetlane są następujące wyniki:

    ```console
    { "ok" : 1 }
    ```

1. Zdefiniować schemat `Books` kolekcji i Wstaw dwa dokumenty, używając następującego polecenia:

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    Wyświetlane są następujące wyniki:

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. Wyświetl dokumenty w bazie danych, używając następującego polecenia:

    ```console
    db.Books.find({}).pretty()
    ```

    Wyświetlane są następujące wyniki:

    ```console
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215d"),
      "Name" : "Design Patterns",
      "Price" : 54.93,
      "Category" : "Computers",
      "Author" : "Ralph Johnson"
    }
    {
      "_id" : ObjectId("5bfd996f7b8e48dc15ff215e"),
      "Name" : "Clean Code",
      "Price" : 43.15,
      "Category" : "Computers",
      "Author" : "Robert C. Martin"
    }
    ```

    Schemat dodaje wygenerowany automatycznie `_id` właściwości typu `ObjectId` dla każdego dokumentu.

Baza danych jest gotowy. Możesz rozpocząć tworzenie interfejsu API sieci web platformy ASP.NET Core.

## <a name="create-the-aspnet-core-web-api-project"></a>Utwórz projekt interfejsu API sieci web platformy ASP.NET Core

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Przejdź do **pliku** > **nowe** > **projektu**.
1. Wybierz **aplikacji sieci Web programu ASP.NET Core**, nadaj projektowi nazwę *BooksApi*i kliknij przycisk **OK**.
1. Wybierz **platformy .NET Core** platformy docelowej i **platformy ASP.NET Core 2.2**. Wybierz **API** projektu szablonu, a następnie kliknij przycisk **OK**:
1. Odwiedź stronę [galerii pakietów NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) do określenia najnowszej stabilnej wersji sterownika platformy .NET dla bazy danych MongoDB. W **Konsola Menedżera pakietów** okna, przejdź do katalogu głównego projektu. Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Uruchom następujące polecenia w powłoce poleceń:

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    Nowy projekt interfejsu API sieci web platformy ASP.NET Core, na przeznaczonych dla platformy .NET Core jest wygenerowany i otworzyć w programie Visual Studio Code.

1. Kliknij przycisk **tak** podczas *"BooksApi" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?*  zostanie wyświetlone powiadomienie.
1. Odwiedź stronę [galerii pakietów NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) do określenia najnowszej stabilnej wersji sterownika platformy .NET dla bazy danych MongoDB. Otwórz **zintegrowany Terminal** i przejdź do katalogu głównego projektu. Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. Przejdź do **pliku** > **nowe rozwiązanie** > **platformy .NET Core** > **aplikacji**.
1. Wybierz **internetowego interfejsu API platformy ASP.NET Core** C# projektu szablonu, a następnie kliknij przycisk **dalej**.
1. Wybierz **platformy .NET Core 2.2** z **platformę docelową** listy rozwijanej, a następnie kliknij przycisk **dalej**.
1. Wprowadź *BooksApi* dla **Nazwa projektu**i kliknij przycisk **Utwórz**.
1. W **rozwiązania** konsoli kliknij prawym przyciskiem myszy projekt **zależności** a następnie wybierz węzeł **Dodawanie pakietów**.
1. Wprowadź *MongoDB.Driver* w polu wyszukiwania, wybierz *MongoDB.Driver* pakietu, a następnie kliknij przycisk **Dodaj pakiet**.
1. Kliknij przycisk **Akceptuj** znajdujący się w **akceptacja licencji** okna dialogowego.

---

## <a name="add-a-model"></a>Dodawanie modelu

1. Dodaj *modeli* katalogu głównym projektu.
1. Dodaj `Book` klasy *modeli* katalogu z następującym kodem:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

W klasie poprzedniego `Id` właściwości:

* Jest wymagany do mapowania obiektu środowiska uruchomieniowego języka wspólnego (CLR) do kolekcji usługi MongoDB.
* Jest oznaczony za pomocą `[BsonId]` do wyznaczenia tę właściwość jako klucz podstawowy dokumentu.
* Jest oznaczony za pomocą `[BsonRepresentation(BsonType.ObjectId)]` umożliwia przekazanie parametru jako typu `string` zamiast `ObjectId`. MONGO obsługuje konwersję z `string` do `ObjectId`.

Inne właściwości w klasie jest oznaczony za pomocą `[BsonElement]` atrybutu. Wartość ten atrybut reprezentuje nazwę właściwości w kolekcji usługi MongoDB.

## <a name="add-a-crud-operations-class"></a>Dodaj klasę operacje CRUD

1. Dodaj *usług* katalogu głównym projektu.
1. Dodaj `BookService` klasy *usług* katalogu z następującym kodem:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. Dodaj parametry połączenia bazy danych MongoDB do *appsettings.json*:

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    Poprzedni `BookstoreDb` dostęp do właściwości w `BookService` konstruktora klasy.

1. W `Startup.ConfigureServices`, zarejestruj `BookService` klasy przy użyciu systemu wstrzykiwanie zależności:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    Poprzedni rejestracji usługi jest niezbędne do obsługi iniekcji konstruktora w korzystających z tych klas.

`BookService` Klasa używa następujących `MongoDB.Driver` elementy członkowskie do wykonywania operacji CRUD w odniesieniu do bazy danych:

* `MongoClient` &ndash; Odczytuje wystąpienie serwera do wykonywania operacji w bazie danych. Konstruktor obiektu tej klasy znajduje się ciąg połączenia bazy danych MongoDB:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* `IMongoDatabase` &ndash; Reprezentuje bazą danych Mongo do wykonywania operacji. W tym samouczku korzysta z ogólnego `GetCollection<T>(collection)` metoda w interfejsie, aby uzyskać dostęp do danych w określonej kolekcji. Po ta metoda jest wywoływana, można wykonać operacji CRUD względem kolekcji. W `GetCollection<T>(collection)` wywołanie metody:
  * `collection` reprezentuje nazwę kolekcji.
  * `T` reprezentuje typ obiektu CLR, które są przechowywane w kolekcji.

`GetCollection<T>(collection)` Zwraca `MongoCollection` obiekt reprezentujący kolekcję. W tym samouczku następujące metody są wywoływane w kolekcji:

* `Find<T>` &ndash; Zwraca wszystkie dokumenty w kolekcji, zgodne z kryteriami wyszukiwania podana.
* `InsertOne` &ndash; Wstawia podany obiekt jako nowy dokument w kolekcji.
* `ReplaceOne` &ndash; Zastępuje jednolitego dokumentu, które są zgodne z kryteriami wyszukiwania podanej za pomocą udostępnionego obiektu.
* `DeleteOne` &ndash; Usuwa pojedynczy dokument zgodne z kryteriami wyszukiwania podana.

## <a name="add-a-controller"></a>Dodawanie kontrolera

1. Dodaj `BooksController` klasy *kontrolerów* katalogu z następującym kodem:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    Poprzedni kontroler internetowego interfejsu API:

    * Używa `BookService` klasy w celu wykonywania operacji CRUD.
    * Zawiera metody akcji w celu obsługi żądań GET, POST, PUT i DELETE protokołu HTTP.
    * <xref:System.Web.Http.ApiController.CreatedAtRoute*> Metoda zwraca odpowiedź 201, która jest standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze. `CreatedAtRoute` również dodaje do odpowiedzi nagłówek lokalizacji. Nagłówek Location określa identyfikator URI nowo utworzonego zadania do wykonania. Zobacz [10.2.2 201 utworzone](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
1. Skompiluj i uruchom aplikację.
1. Przejdź do `http://localhost:<port>/api/books` w przeglądarce. Wyświetlane są następujące odpowiedź w formacie JSON:

    ```json
    [
      {
        "id":"5bfd996f7b8e48dc15ff215d",
        "bookName":"Design Patterns",
        "price":54.93,
        "category":"Computers",
        "author":"Ralph Johnson"
      },
      {
        "id":"5bfd996f7b8e48dc15ff215e",
        "bookName":"Clean Code",
        "price":43.15,
        "category":"Computers",
        "author":"Robert C. Martin"
      }
    ]
    ```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących tworzenia interfejsów API sieci web platformy ASP.NET Core zobacz następujące zasoby:

* [Youtube wersję tego artykułu](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
