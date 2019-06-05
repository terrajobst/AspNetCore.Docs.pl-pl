---
title: Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i usługi MongoDB
author: prkhandelwal
description: Ten samouczek przedstawia sposób tworzenia sieci web platformy ASP.NET Core interfejsu API przy użyciu bazy danych NoSQL bazy danych MongoDB.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 06/04/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 6a8c5d75f562b38015101e039a2f5d96a5491595
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692553"
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
1. Wybierz **aplikacji sieci Web programu ASP.NET Core** typ projektu, a następnie wybierz **dalej**.
1. Nadaj projektowi nazwę *BooksApi*i wybierz **Utwórz**.
1. Wybierz **platformy .NET Core** platformy docelowej i **platformy ASP.NET Core 2.2**. Wybierz **API** projektu szablonu, a następnie wybierz **Utwórz**.
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

1. Po technologię OmniSharp pasek stanu gaśniczego ikona zmieni kolor na zielony, okno dialogowe prosi **"BooksApi" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?** . Wybierz **tak**.
1. Odwiedź stronę [galerii pakietów NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) do określenia najnowszej stabilnej wersji sterownika platformy .NET dla bazy danych MongoDB. Otwórz **zintegrowany Terminal** i przejdź do katalogu głównego projektu. Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. Przejdź do **pliku** > **nowe rozwiązanie** > **platformy .NET Core** > **aplikacji**.
1. Wybierz **internetowego interfejsu API platformy ASP.NET Core** C# projektu szablonu, a następnie wybierz **dalej**.
1. Wybierz **platformy .NET Core 2.2** z **platformę docelową** listy rozwijanej i wybierz pozycję **dalej**.
1. Wprowadź *BooksApi* dla **Nazwa projektu**i wybierz **Utwórz**.
1. W **rozwiązania** konsoli kliknij prawym przyciskiem myszy projekt **zależności** a następnie wybierz węzeł **Dodawanie pakietów**.
1. Wprowadź *MongoDB.Driver* w polu wyszukiwania, wybierz *MongoDB.Driver* pakietu, a następnie wybierz pozycję **Dodaj pakiet**.
1. Wybierz **Akceptuj** znajdujący się w **akceptacja licencji** okna dialogowego.

---

## <a name="add-an-entity-model"></a>Dodawanie modelu jednostki

1. Dodaj *modeli* katalogu głównym projektu.
1. Dodaj `Book` klasy *modeli* katalogu z następującym kodem:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

    W klasie poprzedniego `Id` właściwości:
    
    * Jest wymagany do mapowania obiektu środowiska uruchomieniowego języka wspólnego (CLR) do kolekcji usługi MongoDB.
    * Jest oznaczony za pomocą [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) można określić tę właściwość jako klucz podstawowy dokumentu.
    * Jest oznaczony za pomocą [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) umożliwia przekazanie parametru jako typu `string` zamiast [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) struktury. MONGO obsługuje konwersję z `string` do `ObjectId`.
    
    Inne właściwości w klasie jest oznaczony za pomocą [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) atrybutu. Wartość ten atrybut reprezentuje nazwę właściwości w kolekcji usługi MongoDB.

## <a name="add-a-configuration-model"></a>Dodaj model konfiguracji

1. Dodaj następujące wartości konfiguracji bazy danych do *appsettings.json*:

    [!code-json[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-6)]

1. Dodaj *BookstoreDatabaseSettings.cs* plik *modeli* katalogu z następującym kodem:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/BookstoreDatabaseSettings.cs)]

    Poprzedni `BookstoreDatabaseSettings` klasa jest używana do przechowywania *appsettings.json* pliku `BookstoreDatabaseSettings` wartości właściwości. Za pomocą pliku JSON i C# nazwy właściwości są nazywane tak samo, aby ułatwić proces mapowania.

1. Dodaj następujący kod do `Startup.ConfigureServices`, przed wywołaniem do `AddMvc`:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureDatabaseSettings)]

    W poprzednim kodzie:

    * Wystąpienia konfiguracji, do którego *appsettings.json* pliku `BookstoreDatabaseSettings` powiązań sekcji jest zarejestrowany w kontenerze wstrzykiwanie zależności (DI). Na przykład `BookstoreDatabaseSettings` obiektu `ConnectionString` właściwość jest wypełniana przy użyciu `BookstoreDatabaseSettings:ConnectionString` właściwość *appsettings.json*.
    * `IBookstoreDatabaseSettings` Interfejs jest zarejestrowany w DI za pomocą pojedynczego [okres istnienia usługi](xref:fundamentals/dependency-injection#service-lifetimes). Gdy dodane wystąpienie interfejsu jest rozpoznawana jako `BookstoreDatabaseSettings` obiektu.

1. Dodaj następujący kod na górze *Startup.cs* rozpoznać `BookstoreDatabaseSettings` i `IBookstoreDatabaseSettings` odwołania:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a>Dodawanie obsługi operacji CRUD

1. Dodaj *usług* katalogu głównym projektu.
1. Dodaj `BookService` klasy *usług* katalogu z następującym kodem:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

    W poprzednim kodzie `IBookstoreDatabaseSettings` wystąpienia jest pobierana z DI przy użyciu iniekcji konstruktora. Ta metoda zapewnia dostęp do *appsettings.json* wartości konfiguracji, które zostały dodane w [Dodaj model konfiguracji](#add-a-configuration-model) sekcji.

1. W `Startup.ConfigureServices`, zarejestruj `BookService` klasie z atrybutem DI:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=9)]

    W poprzednim kodzie `BookService` klasy jest zarejestrowane w usłudze DI w celu obsługi iniekcji konstruktora w korzystających z tych klas. Okres istnienia usługi singleton jest najbardziej odpowiednia ponieważ `BookService` zdobywa bezpośredniej zależności `MongoClient`. Na official będzie przydatna [wytycznych ponowne użycie klienta Mongo](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` powinien być zarejestrowany w DI o okresie istnienia usługi singleton.

1. Dodaj następujący kod na górze *Startup.cs* rozpoznać `BookService` odwołania:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiServices)]

`BookService` Klasa używa następujących `MongoDB.Driver` elementy członkowskie do wykonywania operacji CRUD w odniesieniu do bazy danych:

* [Klucza MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; odczytuje wystąpienie serwera do wykonywania operacji w bazie danych. Konstruktor obiektu tej klasy znajduje się ciąg połączenia bazy danych MongoDB:

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* [IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; reprezentuje bazą danych Mongo do wykonywania operacji. W tym samouczku korzysta z ogólnego [GetCollection<TDocument>(kolekcję)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) metodę interfejsu, aby uzyskać dostęp do danych w określonej kolekcji. Wykonywanie operacji CRUD względem kolekcji należy wykonać po ta metoda jest wywoływana. W `GetCollection<TDocument>(collection)` wywołanie metody:
  * `collection` reprezentuje nazwę kolekcji.
  * `TDocument` reprezentuje typ obiektu CLR, które są przechowywane w kolekcji.

`GetCollection<TDocument>(collection)` Zwraca [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) obiekt reprezentujący kolekcję. W tym samouczku następujące metody są wywoływane w kolekcji:

* [DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; usuwa zgodne z kryteriami wyszukiwania podana pojedynczego dokumentu.
* [Znajdź\<TDocument >](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; zwraca wszystkie dokumenty w kolekcji, zgodne z kryteriami wyszukiwania podana.
* [InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Wstawia podany obiekt jako nowy dokument w kolekcji.
* [ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; zastępuje jednolitego dokumentu, które są zgodne z kryteriami wyszukiwania podanej za pomocą udostępnionego obiektu.

## <a name="add-a-controller"></a>Dodawanie kontrolera

Dodaj `BooksController` klasy *kontrolerów* katalogu z następującym kodem:

[!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

Poprzedni kontroler internetowego interfejsu API:

* Używa `BookService` klasy w celu wykonywania operacji CRUD.
* Zawiera metody akcji w celu obsługi żądań GET, POST, PUT i DELETE protokołu HTTP.
* Wywołania <xref:System.Web.Http.ApiController.CreatedAtRoute*> w `Create` metody akcji do zwrócenia [201 protokołu HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) odpowiedzi. Kod stanu 201 to standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze. `CreatedAtRoute` dodaje także `Location` nagłówka odpowiedzi. `Location` Nagłówek Określa identyfikator URI nowo utworzonego książki.

## <a name="test-the-web-api"></a>Testowanie interfejsu API sieci web

1. Skompiluj i uruchom aplikację.

1. Przejdź do `http://localhost:<port>/api/books` do kontrolera testowego użytkownika bez parametrów `Get` metody akcji. Wyświetlane są następujące odpowiedź w formacie JSON:

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

1. Przejdź do `http://localhost:<port>/api/books/5bfd996f7b8e48dc15ff215e` do kontrolera testowego użytkownika przeciążone `Get` metody akcji. Wyświetlane są następujące odpowiedź w formacie JSON:

    ```json
    {
      "id":"5bfd996f7b8e48dc15ff215e",
      "bookName":"Clean Code",
      "price":43.15,
      "category":"Computers",
      "author":"Robert C. Martin"
    }
    ```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących tworzenia interfejsów API sieci web platformy ASP.NET Core zobacz następujące zasoby:

* [YouTube wersję tego artykułu](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
