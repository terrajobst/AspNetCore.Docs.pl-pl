---
title: Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i usługi MongoDB
author: prkhandelwal
description: W tym samouczku przedstawiono sposób tworzenia ASP.NET Core internetowego interfejsu API przy użyciu bazy danych NoSQL MongoDB.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 08/17/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 1425abbfc7bce6bdc445f4e41d9e004405c96e13
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880336"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a>Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i usługi MongoDB

Przez [Pratik Khandelwal](https://twitter.com/K2Prk) i [Scott Addie](https://twitter.com/Scott_Addie)

::: moniker range=">= aspnetcore-3.0"

Ten samouczek tworzy internetowego interfejsu API, która wykonuje operacje tworzenia, odczytu, aktualizowania lub usuwania (CRUD) [bazy danych MongoDB](https://www.mongodb.com/what-is-mongodb) bazy danych NoSQL.

Z tego samouczka dowiesz się, jak wykonywać następujące czynności:

> [!div class="checklist"]
> * Konfigurowanie bazy danych MongoDB
> * Tworzenie bazy danych MongoDB
> * Definiowanie kolekcji usługi MongoDB i schematu
> * Wykonywanie operacji CRUD bazy danych MongoDB z internetowego interfejsu API
> * Dostosowywanie serializacji JSON

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Zestaw .NET Core SDK 3.0 lub nowszy](https://www.microsoft.com/net/download/all)
* [Program Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z **ASP.NET i programowaniem aplikacji sieci Web**
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Zestaw .NET Core SDK 3.0 lub nowszy](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [Środowisko C# dla programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [MongoDB](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* [Zestaw .NET Core SDK 3.0 lub nowszy](https://www.microsoft.com/net/download/all)
* [Program Visual Studio dla komputerów Mac w wersji 7,7 lub nowszy](https://visualstudio.microsoft.com/downloads/)
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a>Konfigurowanie bazy danych MongoDB

W przypadku korzystania z systemu Windows MongoDB jest instalowany w lokalizacji *C:\\Program Files\\* domyślnie. Dodaj *plik C:\\programów\\MongoDB\\Server\\\<* version_number >\\bin do zmiennej środowiskowej `Path`. Dzięki tej zmianie bazy danych MongoDB dostęp z dowolnego miejsca na komputerze deweloperskim.

Użyj powłoki mongo w poniższych krokach umożliwia tworzenie bazy danych, wprowadzanie kolekcje i przechowywanie dokumentów. Aby uzyskać więcej informacji na temat poleceń powłoki mongo, zobacz [Praca z powłoki mongo](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).

1. Wybierz katalog na komputerze deweloperskim do przechowywania danych. Na przykład *C:\\BooksData* w systemie Windows. Utwórz katalog, jeśli nie istnieje. Powłoki mongo nie tworzyć nowe katalogi.
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
  
   > [!NOTE]
   > Identyfikator przedstawiony w tym artykule nie będzie pasował do identyfikatorów podczas uruchamiania tego przykładu.

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

1. Przejdź do **pliku** > **nowym** > **projekcie**.
1. Wybierz typ projektu **aplikacja sieci Web ASP.NET Core** a następnie wybierz przycisk **dalej**.
1. Nazwij projekt *BooksApi*, a następnie wybierz pozycję **Utwórz**.
1. Wybierz platformę docelową **.NET Core** i **ASP.NET Core 3,0**. Wybierz szablon projektu **interfejsu API** i wybierz pozycję **Utwórz**.
1. Odwiedź [galerię NuGet: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) , aby określić najnowszą stabilną wersję sterownika .NET dla usługi MongoDB. W **Konsola Menedżera pakietów** okna, przejdź do katalogu głównego projektu. Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:

   ```powershell
   Install-Package MongoDB.Driver -Version {VERSION}
   ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Uruchom następujące polecenia w powłoce poleceń:

   ```dotnetcli
   dotnet new webapi -o BooksApi
   code BooksApi
   ```

   Nowy projekt interfejsu API sieci web platformy ASP.NET Core, na przeznaczonych dla platformy .NET Core jest wygenerowany i otworzyć w programie Visual Studio Code.

1. Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w oknie dialogowym zostanie wyświetlony monit **o podanie wymaganych zasobów do skompilowania i debugowania z elementu "BooksApi". Dodać je?** . Wybierz pozycję **Yes**.
1. Odwiedź [galerię NuGet: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) , aby określić najnowszą stabilną wersję sterownika .NET dla usługi MongoDB. Otwórz **zintegrowany Terminal** i przejdź do katalogu głównego projektu. Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:

   ```dotnetcli
   dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

1. Przejdź do **pliku** > **nowe rozwiązanie** > **.NET Core** > **App**.
1. Wybierz szablon projektu C# **interfejsu API sieci Web ASP.NET Core** i kliknij przycisk **dalej**.
1. Z listy rozwijanej **platforma docelowa** wybierz pozycję **.NET Core 3,0** , a następnie wybierz pozycję **Next (dalej**).
1. Wprowadź *BooksApi* jako **nazwę projektu**, a następnie wybierz pozycję **Utwórz**.
1. W **rozwiązania** konsoli kliknij prawym przyciskiem myszy projekt **zależności** a następnie wybierz węzeł **Dodawanie pakietów**.
1. Wprowadź *MongoDB. Driver* w polu wyszukiwania, wybierz pakiet *MongoDB. Driver* , a następnie wybierz pozycję **Dodaj pakiet**.
1. Wybierz przycisk **Akceptuj** w oknie dialogowym **akceptacji licencji** .

---

## <a name="add-an-entity-model"></a>Dodaj model jednostki

1. Dodaj *modeli* katalogu głównym projektu.
1. Dodaj `Book` klasy *modeli* katalogu z następującym kodem:

   ```csharp
   using MongoDB.Bson;
   using MongoDB.Bson.Serialization.Attributes;

   namespace BooksApi.Models
   {
       public class Book
       {
           [BsonId]
           [BsonRepresentation(BsonType.ObjectId)]
           public string Id { get; set; }

           [BsonElement("Name")]
           public string BookName { get; set; }

           public decimal Price { get; set; }

           public string Category { get; set; }

           public string Author { get; set; }
       }
   }
   ```

   W poprzedniej klasie Właściwość `Id`:

   * Jest wymagany do mapowania obiektu środowiska uruchomieniowego języka wspólnego (CLR) do kolekcji MongoDB.
   * Zawiera adnotację z [`[BsonId]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) , aby wyznaczyć tę właściwość jako klucz podstawowy dokumentu.
   * Ma adnotację z [`[BsonRepresentation(BsonType.ObjectId)]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) , aby umożliwić przekazywanie parametru jako typu `string` zamiast struktury [objectid](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) . Mongo obsługuje konwersję z `string` na `ObjectId`.

   Właściwość `BookName` ma adnotację z atrybutem [`[BsonElement]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) . Wartość atrybutu `Name` reprezentuje nazwę właściwości w kolekcji MongoDB.

## <a name="add-a-configuration-model"></a>Dodaj model konfiguracji

1. Dodaj następujące wartości konfiguracji bazy danych do pliku *appSettings. JSON*:

   [!code-json[](first-mongo-app/samples/3.x/SampleApp/appsettings.json?highlight=2-6)]

1. Dodaj plik *BookstoreDatabaseSettings.cs* do katalogu *models* o następującym kodzie:

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/BookstoreDatabaseSettings.cs)]

   Poprzednia Klasa `BookstoreDatabaseSettings` jest używana do przechowywania wartości właściwości `BookstoreDatabaseSettings` pliku *appSettings. JSON* . Nazwy JSON i C# właściwości są nazywane identycznie, aby uprościć proces mapowania.

1. Dodaj następujący wyróżniony kod do `Startup.ConfigureServices`:

   [!code-csharp[](first-mongo-app/samples_snapshot/3.x/SampleApp/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

   Powyższy kod ma następujące działanie:

   * Wystąpienie konfiguracji, do którego są powiązane sekcja `BookstoreDatabaseSettings` pliku *appSettings. JSON* , jest zarejestrowane w kontenerze iniekcji zależności (di). Na przykład właściwość `ConnectionString` obiektu `BookstoreDatabaseSettings` jest wypełniana właściwością `BookstoreDatabaseSettings:ConnectionString` w pliku *appSettings. JSON*.
   * Interfejs `IBookstoreDatabaseSettings` jest rejestrowany przy użyciu programu DI z pojedynczym [okresem istnienia usługi](xref:fundamentals/dependency-injection#service-lifetimes). Po dowstrzykiwaniu wystąpienie interfejsu jest rozpoznawane jako obiekt `BookstoreDatabaseSettings`.

1. Dodaj następujący kod na początku *Startup.cs* , aby rozwiązać `BookstoreDatabaseSettings` i `IBookstoreDatabaseSettings` odwołania:

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a>Dodawanie usługi operacji CRUD

1. Dodaj *usług* katalogu głównym projektu.
1. Dodaj `BookService` klasy *usług* katalogu z następującym kodem:

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceClass)]

   W powyższym kodzie wystąpienie `IBookstoreDatabaseSettings` jest pobierane z funkcji DI przez iniekcję konstruktora. Ta technika zapewnia dostęp do wartości konfiguracyjnych *appSettings. JSON* , które zostały dodane w sekcji [Dodawanie modelu konfiguracji](#add-a-configuration-model) .

1. Dodaj następujący wyróżniony kod do `Startup.ConfigureServices`:

   [!code-csharp[](first-mongo-app/samples_snapshot/3.x/SampleApp/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

   W poprzednim kodzie Klasa `BookService` jest zarejestrowana przy użyciu funkcji DI, aby obsługiwać iniekcję konstruktora w klasach zużywających. Okres istnienia usługi pojedynczej jest najbardziej odpowiedni, ponieważ `BookService` wykonuje bezpośrednią zależność od `MongoClient`. Zgodnie z oficjalnymi [wytycznymi klienta Mongo](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)należy zarejestrować `MongoClient` w programie di z pojedynczym okresem istnienia usługi.

1. Dodaj następujący kod na górze elementu *Startup.cs* , aby rozwiązać `BookService` odwołanie:

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiServices)]

`BookService` Klasa używa następujących `MongoDB.Driver` elementy członkowskie do wykonywania operacji CRUD w odniesieniu do bazy danych:

* [MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; odczytuje wystąpienie serwera na potrzeby wykonywania operacji bazy danych. Konstruktor obiektu tej klasy znajduje się ciąg połączenia bazy danych MongoDB:

  [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* [IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; reprezentuje bazę danych Mongo do wykonywania operacji. W tym samouczku do uzyskiwania dostępu do danych w określonej kolekcji jest stosowana Metoda ogólna [getcollection\<TDocument > (Collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) . Wykonaj operacje CRUD w odniesieniu do kolekcji po wywołaniu tej metody. W `GetCollection<TDocument>(collection)` wywołanie metody:

  * `collection` reprezentuje nazwę kolekcji.
  * `TDocument` reprezentuje typ obiektu CLR, które są przechowywane w kolekcji.

`GetCollection<TDocument>(collection)` zwraca obiekt [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) reprezentujący kolekcję. W tym samouczku następujące metody są wywoływane w kolekcji:

* [DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; usuwa pojedynczy dokument pasujący do podanych kryteriów wyszukiwania.
* [Znajdź\<TDocument >](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; zwraca wszystkie dokumenty w kolekcji pasujące do podanych kryteriów wyszukiwania.
* [InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Wstawia podany obiekt jako nowy dokument w kolekcji.
* [ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; zastępuje pojedynczy dokument pasujący do podanych kryteriów wyszukiwania z podanym obiektem.

## <a name="add-a-controller"></a>Dodawanie kontrolera

Dodaj `BooksController` klasy *kontrolerów* katalogu z następującym kodem:

[!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Controllers/BooksController.cs)]

Poprzedni kontroler internetowego interfejsu API:

* Używa `BookService` klasy w celu wykonywania operacji CRUD.
* Zawiera metody akcji w celu obsługi żądań GET, POST, PUT i DELETE protokołu HTTP.
* Wywołuje <xref:System.Web.Http.ApiController.CreatedAtRoute*> w metodzie `Create`, aby zwrócić odpowiedź [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) . Kod stanu 201 jest standardową odpowiedzią dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze. `CreatedAtRoute` dodaje również nagłówek `Location` do odpowiedzi. Nagłówek `Location` określa identyfikator URI nowo utworzonej książki.

## <a name="test-the-web-api"></a>Testowanie interfejsu API sieci Web

1. Skompiluj i uruchom aplikację.

1. Przejdź do `http://localhost:<port>/api/books`, aby przetestować bezparametryczne `Get` metody akcji. Wyświetlane są następujące odpowiedź w formacie JSON:

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

1. Przejdź do `http://localhost:<port>/api/books/{id here}`, aby przetestować metodę `Get` akcji przeciążonej kontrolera. Wyświetlane są następujące odpowiedź w formacie JSON:

   ```json
   {
     "id":"{ID}",
     "bookName":"Clean Code",
     "price":43.15,
     "category":"Computers",
     "author":"Robert C. Martin"
   }
   ```

## <a name="configure-json-serialization-options"></a>Konfigurowanie opcji serializacji JSON

Istnieją dwa szczegóły dotyczące odpowiedzi JSON zwróconych w sekcji [testowanie interfejsu API sieci Web](#test-the-web-api) :

* Nazwy właściwości "default notacji CamelCase wielkość liter należy zmienić tak, aby pasowały do wielkości liter w języku Pascal nazw właściwości obiektu CLR.
* Właściwość `bookName` powinna zostać zwrócona jako `Name`.

Aby spełnić powyższe wymagania, należy wprowadzić następujące zmiany:

1. JSON.NET został usunięty z ASP.NET udostępnionej platformy. Dodaj odwołanie do pakietu do [Microsoft. AspNetCore. MVC. NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson).

1. W `Startup.ConfigureServices`łańcuchować następujący wyróżniony kod do wywołania metody `AddMvc`:

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

   W przypadku poprzedniej zmiany nazwy właściwości w serializowanej odpowiedzi JSON interfejsu API sieci Web pasują do odpowiednich nazw właściwości w typie obiektu CLR. Na przykład Serializacja właściwości `Author` klasy `Book` jako `Author`.

1. W *modelach/książka. cs*Dodaj adnotację do właściwości `BookName` z następującym atrybutem [`[JsonProperty]`](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) :

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

   Wartość `[JsonProperty]` atrybutu `Name` reprezentuje nazwę właściwości w serializowanej odpowiedzi JSON internetowego interfejsu API.

1. Dodaj następujący kod na górze *modeli/książek. cs* , aby rozwiązać `[JsonProperty]` odwołanie do atrybutu:

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. Powtórz kroki zdefiniowane w sekcji [testowanie interfejsu API sieci Web](#test-the-web-api) . Zwróć uwagę na różnice w nazwach właściwości JSON.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Ten samouczek tworzy internetowego interfejsu API, która wykonuje operacje tworzenia, odczytu, aktualizowania lub usuwania (CRUD) [bazy danych MongoDB](https://www.mongodb.com/what-is-mongodb) bazy danych NoSQL.

Z tego samouczka dowiesz się, jak wykonywać następujące czynności:

> [!div class="checklist"]
> * Konfigurowanie bazy danych MongoDB
> * Tworzenie bazy danych MongoDB
> * Definiowanie kolekcji usługi MongoDB i schematu
> * Wykonywanie operacji CRUD bazy danych MongoDB z internetowego interfejsu API
> * Dostosowywanie serializacji JSON

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET Core SDK 2.2](https://www.microsoft.com/net/download/all)
* [Program Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z **ASP.NET i programowaniem aplikacji sieci Web**
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET Core SDK 2.2](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [Środowisko C# dla programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [MongoDB](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

* [.NET Core SDK 2.2](https://www.microsoft.com/net/download/all)
* [Program Visual Studio dla komputerów Mac w wersji 7,7 lub nowszy](https://visualstudio.microsoft.com/downloads/)
* [MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a>Konfigurowanie bazy danych MongoDB

W przypadku korzystania z systemu Windows MongoDB jest instalowany w lokalizacji *C:\\Program Files\\* domyślnie. Dodaj *plik C:\\programów\\MongoDB\\Server\\\<* version_number >\\bin do zmiennej środowiskowej `Path`. Dzięki tej zmianie bazy danych MongoDB dostęp z dowolnego miejsca na komputerze deweloperskim.

Użyj powłoki mongo w poniższych krokach umożliwia tworzenie bazy danych, wprowadzanie kolekcje i przechowywanie dokumentów. Aby uzyskać więcej informacji na temat poleceń powłoki mongo, zobacz [Praca z powłoki mongo](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).

1. Wybierz katalog na komputerze deweloperskim do przechowywania danych. Na przykład *C:\\BooksData* w systemie Windows. Utwórz katalog, jeśli nie istnieje. Powłoki mongo nie tworzyć nowe katalogi.
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
  
   > [!NOTE]
   > Identyfikator przedstawiony w tym artykule nie będzie pasował do identyfikatorów podczas uruchamiania tego przykładu.

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

1. Przejdź do **pliku** > **nowym** > **projekcie**.
1. Wybierz typ projektu **aplikacja sieci Web ASP.NET Core** a następnie wybierz przycisk **dalej**.
1. Nazwij projekt *BooksApi*, a następnie wybierz pozycję **Utwórz**.
1. Wybierz platformę docelową **.NET Core** i **ASP.NET Core 2,2**. Wybierz szablon projektu **interfejsu API** i wybierz pozycję **Utwórz**.
1. Odwiedź [galerię NuGet: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) , aby określić najnowszą stabilną wersję sterownika .NET dla usługi MongoDB. W **Konsola Menedżera pakietów** okna, przejdź do katalogu głównego projektu. Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:

   ```powershell
   Install-Package MongoDB.Driver -Version {VERSION}
   ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Uruchom następujące polecenia w powłoce poleceń:

   ```dotnetcli
   dotnet new webapi -o BooksApi
   code BooksApi
   ```

   Nowy projekt interfejsu API sieci web platformy ASP.NET Core, na przeznaczonych dla platformy .NET Core jest wygenerowany i otworzyć w programie Visual Studio Code.

1. Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w oknie dialogowym zostanie wyświetlony monit **o podanie wymaganych zasobów do skompilowania i debugowania z elementu "BooksApi". Dodać je?** . Wybierz pozycję **Yes**.
1. Odwiedź [galerię NuGet: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) , aby określić najnowszą stabilną wersję sterownika .NET dla usługi MongoDB. Otwórz **zintegrowany Terminal** i przejdź do katalogu głównego projektu. Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:

   ```dotnetcli
   dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

1. Przejdź do **pliku** > **nowe rozwiązanie** > **.NET Core** > **App**.
1. Wybierz szablon projektu C# **interfejsu API sieci Web ASP.NET Core** i kliknij przycisk **dalej**.
1. Z listy rozwijanej **platforma docelowa** wybierz pozycję **.NET Core 2,2** , a następnie wybierz pozycję **Next (dalej**).
1. Wprowadź *BooksApi* jako **nazwę projektu**, a następnie wybierz pozycję **Utwórz**.
1. W **rozwiązania** konsoli kliknij prawym przyciskiem myszy projekt **zależności** a następnie wybierz węzeł **Dodawanie pakietów**.
1. Wprowadź *MongoDB. Driver* w polu wyszukiwania, wybierz pakiet *MongoDB. Driver* , a następnie wybierz pozycję **Dodaj pakiet**.
1. Wybierz przycisk **Akceptuj** w oknie dialogowym **akceptacji licencji** .

---

## <a name="add-an-entity-model"></a>Dodaj model jednostki

1. Dodaj *modeli* katalogu głównym projektu.
1. Dodaj `Book` klasy *modeli* katalogu z następującym kodem:

   ```csharp
   using MongoDB.Bson;
   using MongoDB.Bson.Serialization.Attributes;

   namespace BooksApi.Models
   {
       public class Book
       {
           [BsonId]
           [BsonRepresentation(BsonType.ObjectId)]
           public string Id { get; set; }

           [BsonElement("Name")]
           public string BookName { get; set; }

           public decimal Price { get; set; }

           public string Category { get; set; }

           public string Author { get; set; }
       }
   }
   ```

   W poprzedniej klasie Właściwość `Id`:

   * Jest wymagany do mapowania obiektu środowiska uruchomieniowego języka wspólnego (CLR) do kolekcji MongoDB.
   * Zawiera adnotację z [`[BsonId]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) , aby wyznaczyć tę właściwość jako klucz podstawowy dokumentu.
   * Ma adnotację z [`[BsonRepresentation(BsonType.ObjectId)]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) , aby umożliwić przekazywanie parametru jako typu `string` zamiast struktury [objectid](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) . Mongo obsługuje konwersję z `string` na `ObjectId`.

   Właściwość `BookName` ma adnotację z atrybutem [`[BsonElement]`](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) . Wartość atrybutu `Name` reprezentuje nazwę właściwości w kolekcji MongoDB.

## <a name="add-a-configuration-model"></a>Dodaj model konfiguracji

1. Dodaj następujące wartości konfiguracji bazy danych do pliku *appSettings. JSON*:

   [!code-json[](first-mongo-app/samples/2.x/SampleApp/appsettings.json?highlight=2-6)]

1. Dodaj plik *BookstoreDatabaseSettings.cs* do katalogu *models* o następującym kodzie:

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/BookstoreDatabaseSettings.cs)]

   Poprzednia Klasa `BookstoreDatabaseSettings` jest używana do przechowywania wartości właściwości `BookstoreDatabaseSettings` pliku *appSettings. JSON* . Nazwy JSON i C# właściwości są nazywane identycznie, aby uprościć proces mapowania.

1. Dodaj następujący wyróżniony kod do `Startup.ConfigureServices`:

   [!code-csharp[](first-mongo-app/samples_snapshot/2.x/SampleApp/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

   Powyższy kod ma następujące działanie:

   * Wystąpienie konfiguracji, do którego są powiązane sekcja `BookstoreDatabaseSettings` pliku *appSettings. JSON* , jest zarejestrowane w kontenerze iniekcji zależności (di). Na przykład właściwość `ConnectionString` obiektu `BookstoreDatabaseSettings` jest wypełniana właściwością `BookstoreDatabaseSettings:ConnectionString` w pliku *appSettings. JSON*.
   * Interfejs `IBookstoreDatabaseSettings` jest rejestrowany przy użyciu programu DI z pojedynczym [okresem istnienia usługi](xref:fundamentals/dependency-injection#service-lifetimes). Po dowstrzykiwaniu wystąpienie interfejsu jest rozpoznawane jako obiekt `BookstoreDatabaseSettings`.

1. Dodaj następujący kod na początku *Startup.cs* , aby rozwiązać `BookstoreDatabaseSettings` i `IBookstoreDatabaseSettings` odwołania:

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a>Dodawanie usługi operacji CRUD

1. Dodaj *usług* katalogu głównym projektu.
1. Dodaj `BookService` klasy *usług* katalogu z następującym kodem:

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceClass)]

   W powyższym kodzie wystąpienie `IBookstoreDatabaseSettings` jest pobierane z funkcji DI przez iniekcję konstruktora. Ta technika zapewnia dostęp do wartości konfiguracyjnych *appSettings. JSON* , które zostały dodane w sekcji [Dodawanie modelu konfiguracji](#add-a-configuration-model) .

1. Dodaj następujący wyróżniony kod do `Startup.ConfigureServices`:

   [!code-csharp[](first-mongo-app/samples_snapshot/2.x/SampleApp/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

   W poprzednim kodzie Klasa `BookService` jest zarejestrowana przy użyciu funkcji DI, aby obsługiwać iniekcję konstruktora w klasach zużywających. Okres istnienia usługi pojedynczej jest najbardziej odpowiedni, ponieważ `BookService` wykonuje bezpośrednią zależność od `MongoClient`. Zgodnie z oficjalnymi [wytycznymi klienta Mongo](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)należy zarejestrować `MongoClient` w programie di z pojedynczym okresem istnienia usługi.

1. Dodaj następujący kod na górze elementu *Startup.cs* , aby rozwiązać `BookService` odwołanie:

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiServices)]

`BookService` Klasa używa następujących `MongoDB.Driver` elementy członkowskie do wykonywania operacji CRUD w odniesieniu do bazy danych:

* [MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; odczytuje wystąpienie serwera na potrzeby wykonywania operacji bazy danych. Konstruktor obiektu tej klasy znajduje się ciąg połączenia bazy danych MongoDB:

  [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* [IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; reprezentuje bazę danych Mongo do wykonywania operacji. W tym samouczku do uzyskiwania dostępu do danych w określonej kolekcji jest stosowana Metoda ogólna [getcollection\<TDocument > (Collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) . Wykonaj operacje CRUD w odniesieniu do kolekcji po wywołaniu tej metody. W `GetCollection<TDocument>(collection)` wywołanie metody:

  * `collection` reprezentuje nazwę kolekcji.
  * `TDocument` reprezentuje typ obiektu CLR, które są przechowywane w kolekcji.

`GetCollection<TDocument>(collection)` zwraca obiekt [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) reprezentujący kolekcję. W tym samouczku następujące metody są wywoływane w kolekcji:

* [DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; usuwa pojedynczy dokument pasujący do podanych kryteriów wyszukiwania.
* [Znajdź\<TDocument >](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; zwraca wszystkie dokumenty w kolekcji pasujące do podanych kryteriów wyszukiwania.
* [InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Wstawia podany obiekt jako nowy dokument w kolekcji.
* [ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; zastępuje pojedynczy dokument pasujący do podanych kryteriów wyszukiwania z podanym obiektem.

## <a name="add-a-controller"></a>Dodawanie kontrolera

Dodaj `BooksController` klasy *kontrolerów* katalogu z następującym kodem:

[!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Controllers/BooksController.cs)]

Poprzedni kontroler internetowego interfejsu API:

* Używa `BookService` klasy w celu wykonywania operacji CRUD.
* Zawiera metody akcji w celu obsługi żądań GET, POST, PUT i DELETE protokołu HTTP.
* Wywołuje <xref:System.Web.Http.ApiController.CreatedAtRoute*> w metodzie `Create`, aby zwrócić odpowiedź [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) . Kod stanu 201 jest standardową odpowiedzią dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze. `CreatedAtRoute` dodaje również nagłówek `Location` do odpowiedzi. Nagłówek `Location` określa identyfikator URI nowo utworzonej książki.

## <a name="test-the-web-api"></a>Testowanie interfejsu API sieci Web

1. Skompiluj i uruchom aplikację.

1. Przejdź do `http://localhost:<port>/api/books`, aby przetestować bezparametryczne `Get` metody akcji. Wyświetlane są następujące odpowiedź w formacie JSON:

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

1. Przejdź do `http://localhost:<port>/api/books/{id here}`, aby przetestować metodę `Get` akcji przeciążonej kontrolera. Wyświetlane są następujące odpowiedź w formacie JSON:

   ```json
   {
     "id":"{ID}",
     "bookName":"Clean Code",
     "price":43.15,
     "category":"Computers",
     "author":"Robert C. Martin"
   }
   ```

## <a name="configure-json-serialization-options"></a>Konfigurowanie opcji serializacji JSON

Istnieją dwa szczegóły dotyczące odpowiedzi JSON zwróconych w sekcji [testowanie interfejsu API sieci Web](#test-the-web-api) :

* Nazwy właściwości "default notacji CamelCase wielkość liter należy zmienić tak, aby pasowały do wielkości liter w języku Pascal nazw właściwości obiektu CLR.
* Właściwość `bookName` powinna zostać zwrócona jako `Name`.

Aby spełnić powyższe wymagania, należy wprowadzić następujące zmiany:

1. W `Startup.ConfigureServices`łańcuchować następujący wyróżniony kod do wywołania metody `AddMvc`:

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

   W przypadku poprzedniej zmiany nazwy właściwości w serializowanej odpowiedzi JSON interfejsu API sieci Web pasują do odpowiednich nazw właściwości w typie obiektu CLR. Na przykład Serializacja właściwości `Author` klasy `Book` jako `Author`.

1. W *modelach/książka. cs*Dodaj adnotację do właściwości `BookName` z następującym atrybutem [`[JsonProperty]`](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) :

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

   Wartość `[JsonProperty]` atrybutu `Name` reprezentuje nazwę właściwości w serializowanej odpowiedzi JSON internetowego interfejsu API.

1. Dodaj następujący kod na górze *modeli/książek. cs* , aby rozwiązać `[JsonProperty]` odwołanie do atrybutu:

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. Powtórz kroki zdefiniowane w sekcji [testowanie interfejsu API sieci Web](#test-the-web-api) . Zwróć uwagę na różnice w nazwach właściwości JSON.

::: moniker-end

## <a name="add-authentication-support-to-a-web-api"></a>Dodawanie obsługi uwierzytelniania do internetowego interfejsu API

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących tworzenia interfejsów API sieci web platformy ASP.NET Core zobacz następujące zasoby:

* [Wersja tego artykułu usługi YouTube](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
