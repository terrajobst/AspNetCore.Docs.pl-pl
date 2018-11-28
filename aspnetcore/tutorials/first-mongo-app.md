---
title: Tworzenie interfejsów API sieci web za pomocą platformy ASP.NET Core i bazy danych MongoDB
author: pratik-khandelwal
description: W tym samouczku przedstawiono sposób tworzenia sieci web platformy ASP.NET Core interfejsu API przy użyciu bazy danych NoSQL bazy danych MongoDB.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/26/2018
uid: tutorials/first-mongo-app
ms.openlocfilehash: c4e00eeb2c4aecde9c70c6902e21d06853be7696
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458496"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a>Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i usługi MongoDB

Przez [Pratik Khandelwal](https://twitter.com/K2Prk) i [Scott Addie](https://twitter.com/Scott_Addie)

Ten samouczek tworzy internetowego interfejsu API, która wykonuje operacje tworzenia, odczytu, aktualizowania lub usuwania (CRUD) [bazy danych MongoDB](https://www.mongodb.com/what-is-mongodb) bazy danych NoSQL.

W tym samouczku dowiesz się, jak:

> [!div class="checklist"]
> * Konfigurowanie bazy danych MongoDB
> * Tworzenie bazy danych MongoDB
> * Definiowanie kolekcji usługi MongoDB i schematu
> * Wykonywanie operacji CRUD bazy danych MongoDB z internetowego interfejsu API

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Wymagania wstępne

* [.NET core SDK 2.1 lub nowszej](https://www.microsoft.com/net/download/all)
* [Bazy danych MongoDB](https://docs.mongodb.com/manual/administration/install-community/)
* [Program Visual Studio 2017](https://www.visualstudio.com/downloads/) wersji 15.7.3 lub nowszy z następującymi pakietami roboczymi:
  * **Programowanie dla wielu platform .NET core**
  * **ASP.NET i tworzenie aplikacji internetowych**

## <a name="configure-mongodb"></a>Konfigurowanie bazy danych MongoDB

Jeśli przy użyciu Windows, bazy danych MongoDB jest zainstalowana w *C:\Program Files\MongoDB* domyślnie. Dodaj *C:\Program Files\MongoDB\Server\<numer_wersji > \bin* do `Path` zmiennej środowiskowej. Dzięki tej zmianie bazy danych MongoDB dostęp z dowolnego miejsca na komputerze deweloperskim.

Użyj powłoki mongo w poniższych krokach umożliwia tworzenie bazy danych, wprowadzanie kolekcje i przechowywanie dokumentów. Aby uzyskać więcej informacji na temat poleceń powłoki mongo, zobacz [Praca z powłoki mongo](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).

1. Wybierz katalog na komputerze deweloperskim do przechowywania danych. Na przykład *C:\BooksData* na Windows. Utwórz katalog, jeśli nie istnieje. Powłoki mongo nie tworzyć nowe katalogi.
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

1. W programie Visual Studio, przejdź do **pliku** > **New** > **projektu**.
1. Wybierz **aplikacji sieci Web programu ASP.NET Core**, nadaj projektowi nazwę *BookMongo*i kliknij przycisk **OK**.
1. Wybierz **platformy .NET Core** platformy docelowej i **platformy ASP.NET Core 2.1**. Wybierz **API** projektu szablonu, a następnie kliknij przycisk **OK**:
1. W **Konsola Menedżera pakietów** okna, przejdź do katalogu głównego projektu. Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:

    ```powershell
    Install-Package MongoDB.Driver -Version 2.7.2
    ```

## <a name="add-a-model"></a>Dodawanie modelu

1. Dodaj *modeli* folder w katalogu głównym projektu.
1. Dodaj `Book` klasy *modeli* folderu z następującym kodem:

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Models/Book.cs)]

W klasie poprzedniego `Id` właściwość jest wymagana do mapowania obiektu środowiska uruchomieniowego języka wspólnego (CLR) do kolekcji usługi MongoDB. Inne właściwości w klasie są ozdobione `[BsonElement]` atrybutu. Wartość ten atrybut reprezentuje nazwę właściwości w kolekcji usługi MongoDB.

## <a name="add-a-crud-operations-class"></a>Dodaj klasę operacje CRUD

1. Dodaj *usług* folder w katalogu głównym projektu.
1. Dodaj `BookService` klasy *usług* folderu z następującym kodem:

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Services/BookService.cs?name=snippet_BookServiceClass)]

1. Dodaj parametry połączenia bazy danych MongoDB do *appsettings.json*:

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/appsettings.json?highlight=2-4)]

    Poprzedni `BookstoreDb` dostęp do właściwości w `BookService` konstruktora klasy.

1. W `Startup.ConfigureServices`, zarejestruj `BookService` klasy przy użyciu systemu wstrzykiwanie zależności:

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    Poprzedni rejestracji usługi jest niezbędne do obsługi iniekcji konstruktora w korzystających z tych klas.

`BookService` Klasa używa następujących `MongoDB.Driver` elementy członkowskie do wykonywania operacji CRUD w odniesieniu do bazy danych:

* `MongoClient` &ndash; Odczytuje wystąpienie serwera do wykonywania operacji w bazie danych. Konstruktor obiektu tej klasy znajduje się ciąg połączenia bazy danych MongoDB:

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* `IMongoDatabase` &ndash; Reprezentuje bazą danych Mongo do wykonywania operacji. W tym samouczku korzysta z ogólnego `GetCollection<T>(collection)` metoda w interfejsie, aby uzyskać dostęp do danych w określonej kolekcji. Po ta metoda jest wywoływana, można wykonać operacji CRUD względem kolekcji. W `GetCollection<T>(collection)` wywołanie metody:
  * `collection` reprezentuje nazwę kolekcji.
  * `T` reprezentuje typ obiektu CLR, które są przechowywane w kolekcji.

`GetCollection<T>(collection)` Zwraca `MongoCollection` obiekt reprezentujący kolekcję. W tym samouczku następujące metody są wywoływane w kolekcji:

* `Find<T>` &ndash; Zwraca wszystkie dokumenty w kolekcji, zgodne z kryteriami wyszukiwania podana.
* `InsertOne` &ndash; Wstawia podany obiekt jako nowy dokument w kolekcji.
* `ReplaceOne` &ndash; Zastępuje jednolitego dokumentu, które są zgodne z kryteriami wyszukiwania podanej za pomocą udostępnionego obiektu.
* `DeleteOne` &ndash; Usuwa pojedynczy dokument zgodne z kryteriami wyszukiwania podana.

## <a name="add-a-controller"></a>Dodawanie kontrolera

1. Kliknij prawym przyciskiem myszy *kontrolerów* folderu w **Eksploratora rozwiązań**. Wybierz **Dodaj** > **kontrolera**.
1. Wybierz **pusty Kontroler interfejsu API —** szablonu elementu, a następnie kliknij przycisk **Dodaj**.
1. Wprowadź *BooksController* w **nazwy kontrolera** pola tekstowego, a następnie kliknij przycisk **Dodaj**.
1. Dodaj następujący kod do *BooksController.cs*:

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Controllers/BooksController.cs)]

    Poprzedni kontroler internetowego interfejsu API:

    * Używa `BookService` klasy w celu wykonywania operacji CRUD.
    * Zawiera metody akcji w celu obsługi żądań GET, POST, PUT i DELETE protokołu HTTP.
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

* <xref:web-api/index>
* <xref:web-api/action-return-types>
