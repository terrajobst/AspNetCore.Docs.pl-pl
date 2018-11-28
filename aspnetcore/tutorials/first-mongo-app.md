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
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="50fca-103">Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i usługi MongoDB</span><span class="sxs-lookup"><span data-stu-id="50fca-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="50fca-104">Przez [Pratik Khandelwal](https://twitter.com/K2Prk) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="50fca-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="50fca-105">Ten samouczek tworzy internetowego interfejsu API, która wykonuje operacje tworzenia, odczytu, aktualizowania lub usuwania (CRUD) [bazy danych MongoDB](https://www.mongodb.com/what-is-mongodb) bazy danych NoSQL.</span><span class="sxs-lookup"><span data-stu-id="50fca-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="50fca-106">W tym samouczku dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="50fca-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="50fca-107">Konfigurowanie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="50fca-107">Configure MongoDB</span></span>
> * <span data-ttu-id="50fca-108">Tworzenie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="50fca-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="50fca-109">Definiowanie kolekcji usługi MongoDB i schematu</span><span class="sxs-lookup"><span data-stu-id="50fca-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="50fca-110">Wykonywanie operacji CRUD bazy danych MongoDB z internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="50fca-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="50fca-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="50fca-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="50fca-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="50fca-112">Prerequisites</span></span>

* [<span data-ttu-id="50fca-113">.NET core SDK 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="50fca-113">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="50fca-114">Bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="50fca-114">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)
* <span data-ttu-id="50fca-115">[Program Visual Studio 2017](https://www.visualstudio.com/downloads/) wersji 15.7.3 lub nowszy z następującymi pakietami roboczymi:</span><span class="sxs-lookup"><span data-stu-id="50fca-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the following workloads:</span></span>
  * <span data-ttu-id="50fca-116">**Programowanie dla wielu platform .NET core**</span><span class="sxs-lookup"><span data-stu-id="50fca-116">**.NET Core cross-platform development**</span></span>
  * <span data-ttu-id="50fca-117">**ASP.NET i tworzenie aplikacji internetowych**</span><span class="sxs-lookup"><span data-stu-id="50fca-117">**ASP.NET and web development**</span></span>

## <a name="configure-mongodb"></a><span data-ttu-id="50fca-118">Konfigurowanie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="50fca-118">Configure MongoDB</span></span>

<span data-ttu-id="50fca-119">Jeśli przy użyciu Windows, bazy danych MongoDB jest zainstalowana w *C:\Program Files\MongoDB* domyślnie.</span><span class="sxs-lookup"><span data-stu-id="50fca-119">If using Windows, MongoDB is installed at *C:\Program Files\MongoDB* by default.</span></span> <span data-ttu-id="50fca-120">Dodaj *C:\Program Files\MongoDB\Server\<numer_wersji > \bin* do `Path` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="50fca-120">Add *C:\Program Files\MongoDB\Server\<version_number>\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="50fca-121">Dzięki tej zmianie bazy danych MongoDB dostęp z dowolnego miejsca na komputerze deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="50fca-121">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="50fca-122">Użyj powłoki mongo w poniższych krokach umożliwia tworzenie bazy danych, wprowadzanie kolekcje i przechowywanie dokumentów.</span><span class="sxs-lookup"><span data-stu-id="50fca-122">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="50fca-123">Aby uzyskać więcej informacji na temat poleceń powłoki mongo, zobacz [Praca z powłoki mongo](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="50fca-123">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="50fca-124">Wybierz katalog na komputerze deweloperskim do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="50fca-124">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="50fca-125">Na przykład *C:\BooksData* na Windows.</span><span class="sxs-lookup"><span data-stu-id="50fca-125">For example, *C:\BooksData* on Windows.</span></span> <span data-ttu-id="50fca-126">Utwórz katalog, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="50fca-126">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="50fca-127">Powłoki mongo nie tworzyć nowe katalogi.</span><span class="sxs-lookup"><span data-stu-id="50fca-127">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="50fca-128">Otwórz powłokę wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="50fca-128">Open a command shell.</span></span> <span data-ttu-id="50fca-129">Uruchom następujące polecenie, aby nawiązać połączenie z bazą danych MongoDB na domyślnym porcie 27017.</span><span class="sxs-lookup"><span data-stu-id="50fca-129">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="50fca-130">Pamiętaj, aby zastąpić `<data_directory_path>` z katalogu, który został wybrany w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="50fca-130">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="50fca-131">Otwórz inne wystąpienie powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="50fca-131">Open another command shell instance.</span></span> <span data-ttu-id="50fca-132">Połączenia z bazą danych testu domyślną, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="50fca-132">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="50fca-133">Uruchom następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="50fca-133">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="50fca-134">Jeśli jeszcze nie istnieje, bazę danych o nazwie *BookstoreDb* zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="50fca-134">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="50fca-135">Jeśli baza danych istnieje, jego połączenie jest otwarte dla transakcji.</span><span class="sxs-lookup"><span data-stu-id="50fca-135">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="50fca-136">Utwórz `Books` kolekcji za pomocą następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="50fca-136">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="50fca-137">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="50fca-137">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="50fca-138">Zdefiniować schemat `Books` kolekcji i Wstaw dwa dokumenty, używając następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="50fca-138">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="50fca-139">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="50fca-139">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="50fca-140">Wyświetl dokumenty w bazie danych, używając następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="50fca-140">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="50fca-141">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="50fca-141">The following result is displayed:</span></span>

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

    <span data-ttu-id="50fca-142">Schemat dodaje wygenerowany automatycznie `_id` właściwości typu `ObjectId` dla każdego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="50fca-142">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="50fca-143">Baza danych jest gotowy.</span><span class="sxs-lookup"><span data-stu-id="50fca-143">The database is ready.</span></span> <span data-ttu-id="50fca-144">Możesz rozpocząć tworzenie interfejsu API sieci web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="50fca-144">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="50fca-145">Utwórz projekt interfejsu API sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="50fca-145">Create the ASP.NET Core web API project</span></span>

1. <span data-ttu-id="50fca-146">W programie Visual Studio, przejdź do **pliku** > **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="50fca-146">In Visual Studio, go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="50fca-147">Wybierz **aplikacji sieci Web programu ASP.NET Core**, nadaj projektowi nazwę *BookMongo*i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="50fca-147">Select **ASP.NET Core Web Application**, name the project *BookMongo*, and click **OK**.</span></span>
1. <span data-ttu-id="50fca-148">Wybierz **platformy .NET Core** platformy docelowej i **platformy ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="50fca-148">Select the **.NET Core** target framework and **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="50fca-149">Wybierz **API** projektu szablonu, a następnie kliknij przycisk **OK**:</span><span class="sxs-lookup"><span data-stu-id="50fca-149">Select the **API** project template, and click **OK**:</span></span>
1. <span data-ttu-id="50fca-150">W **Konsola Menedżera pakietów** okna, przejdź do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="50fca-150">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="50fca-151">Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:</span><span class="sxs-lookup"><span data-stu-id="50fca-151">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version 2.7.2
    ```

## <a name="add-a-model"></a><span data-ttu-id="50fca-152">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="50fca-152">Add a model</span></span>

1. <span data-ttu-id="50fca-153">Dodaj *modeli* folder w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="50fca-153">Add a *Models* folder to the project root.</span></span>
1. <span data-ttu-id="50fca-154">Dodaj `Book` klasy *modeli* folderu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="50fca-154">Add a `Book` class to the *Models* folder with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Models/Book.cs)]

<span data-ttu-id="50fca-155">W klasie poprzedniego `Id` właściwość jest wymagana do mapowania obiektu środowiska uruchomieniowego języka wspólnego (CLR) do kolekcji usługi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="50fca-155">In the preceding class, the `Id` property is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span> <span data-ttu-id="50fca-156">Inne właściwości w klasie są ozdobione `[BsonElement]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="50fca-156">Other properties in the class are decorated with the `[BsonElement]` attribute.</span></span> <span data-ttu-id="50fca-157">Wartość ten atrybut reprezentuje nazwę właściwości w kolekcji usługi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="50fca-157">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-crud-operations-class"></a><span data-ttu-id="50fca-158">Dodaj klasę operacje CRUD</span><span class="sxs-lookup"><span data-stu-id="50fca-158">Add a CRUD operations class</span></span>

1. <span data-ttu-id="50fca-159">Dodaj *usług* folder w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="50fca-159">Add a *Services* folder to the project root.</span></span>
1. <span data-ttu-id="50fca-160">Dodaj `BookService` klasy *usług* folderu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="50fca-160">Add a `BookService` class to the *Services* folder with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Services/BookService.cs?name=snippet_BookServiceClass)]

1. <span data-ttu-id="50fca-161">Dodaj parametry połączenia bazy danych MongoDB do *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="50fca-161">Add the MongoDB connection string to *appsettings.json*:</span></span>

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/appsettings.json?highlight=2-4)]

    <span data-ttu-id="50fca-162">Poprzedni `BookstoreDb` dostęp do właściwości w `BookService` konstruktora klasy.</span><span class="sxs-lookup"><span data-stu-id="50fca-162">The preceding `BookstoreDb` property is accessed in the `BookService` class constructor.</span></span>

1. <span data-ttu-id="50fca-163">W `Startup.ConfigureServices`, zarejestruj `BookService` klasy przy użyciu systemu wstrzykiwanie zależności:</span><span class="sxs-lookup"><span data-stu-id="50fca-163">In `Startup.ConfigureServices`, register the `BookService` class with the Dependency Injection system:</span></span>

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    <span data-ttu-id="50fca-164">Poprzedni rejestracji usługi jest niezbędne do obsługi iniekcji konstruktora w korzystających z tych klas.</span><span class="sxs-lookup"><span data-stu-id="50fca-164">The preceding service registration is necessary to support constructor injection in consuming classes.</span></span>

<span data-ttu-id="50fca-165">`BookService` Klasa używa następujących `MongoDB.Driver` elementy członkowskie do wykonywania operacji CRUD w odniesieniu do bazy danych:</span><span class="sxs-lookup"><span data-stu-id="50fca-165">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="50fca-166">`MongoClient` &ndash; Odczytuje wystąpienie serwera do wykonywania operacji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="50fca-166">`MongoClient` &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="50fca-167">Konstruktor obiektu tej klasy znajduje się ciąg połączenia bazy danych MongoDB:</span><span class="sxs-lookup"><span data-stu-id="50fca-167">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="50fca-168">`IMongoDatabase` &ndash; Reprezentuje bazą danych Mongo do wykonywania operacji.</span><span class="sxs-lookup"><span data-stu-id="50fca-168">`IMongoDatabase` &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="50fca-169">W tym samouczku korzysta z ogólnego `GetCollection<T>(collection)` metoda w interfejsie, aby uzyskać dostęp do danych w określonej kolekcji.</span><span class="sxs-lookup"><span data-stu-id="50fca-169">This tutorial uses the generic `GetCollection<T>(collection)` method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="50fca-170">Po ta metoda jest wywoływana, można wykonać operacji CRUD względem kolekcji.</span><span class="sxs-lookup"><span data-stu-id="50fca-170">CRUD operations can be performed against the collection after this method is called.</span></span> <span data-ttu-id="50fca-171">W `GetCollection<T>(collection)` wywołanie metody:</span><span class="sxs-lookup"><span data-stu-id="50fca-171">In the `GetCollection<T>(collection)` method call:</span></span>
  * <span data-ttu-id="50fca-172">`collection` reprezentuje nazwę kolekcji.</span><span class="sxs-lookup"><span data-stu-id="50fca-172">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="50fca-173">`T` reprezentuje typ obiektu CLR, które są przechowywane w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="50fca-173">`T` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="50fca-174">`GetCollection<T>(collection)` Zwraca `MongoCollection` obiekt reprezentujący kolekcję.</span><span class="sxs-lookup"><span data-stu-id="50fca-174">`GetCollection<T>(collection)` returns a `MongoCollection` object representing the collection.</span></span> <span data-ttu-id="50fca-175">W tym samouczku następujące metody są wywoływane w kolekcji:</span><span class="sxs-lookup"><span data-stu-id="50fca-175">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="50fca-176">`Find<T>` &ndash; Zwraca wszystkie dokumenty w kolekcji, zgodne z kryteriami wyszukiwania podana.</span><span class="sxs-lookup"><span data-stu-id="50fca-176">`Find<T>` &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="50fca-177">`InsertOne` &ndash; Wstawia podany obiekt jako nowy dokument w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="50fca-177">`InsertOne` &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="50fca-178">`ReplaceOne` &ndash; Zastępuje jednolitego dokumentu, które są zgodne z kryteriami wyszukiwania podanej za pomocą udostępnionego obiektu.</span><span class="sxs-lookup"><span data-stu-id="50fca-178">`ReplaceOne` &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>
* <span data-ttu-id="50fca-179">`DeleteOne` &ndash; Usuwa pojedynczy dokument zgodne z kryteriami wyszukiwania podana.</span><span class="sxs-lookup"><span data-stu-id="50fca-179">`DeleteOne` &ndash; Deletes a single document matching the provided search criteria.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="50fca-180">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="50fca-180">Add a controller</span></span>

1. <span data-ttu-id="50fca-181">Kliknij prawym przyciskiem myszy *kontrolerów* folderu w **Eksploratora rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="50fca-181">Right-click the *Controllers* folder in **Solution Explorer**.</span></span> <span data-ttu-id="50fca-182">Wybierz **Dodaj** > **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="50fca-182">Select **Add** > **Controller**.</span></span>
1. <span data-ttu-id="50fca-183">Wybierz **pusty Kontroler interfejsu API —** szablonu elementu, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="50fca-183">Choose the **API Controller - Empty** item template, and click **Add**.</span></span>
1. <span data-ttu-id="50fca-184">Wprowadź *BooksController* w **nazwy kontrolera** pola tekstowego, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="50fca-184">Enter *BooksController* in the **Controller name** text box, and click **Add**.</span></span>
1. <span data-ttu-id="50fca-185">Dodaj następujący kod do *BooksController.cs*:</span><span class="sxs-lookup"><span data-stu-id="50fca-185">Add the following code to *BooksController.cs*:</span></span>

    [!code-csharp[](first-mongo-app/sample/BookstoreAPI/Controllers/BooksController.cs)]

    <span data-ttu-id="50fca-186">Poprzedni kontroler internetowego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="50fca-186">The preceding web API controller:</span></span>

    * <span data-ttu-id="50fca-187">Używa `BookService` klasy w celu wykonywania operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="50fca-187">Uses the `BookService` class to perform CRUD operations.</span></span>
    * <span data-ttu-id="50fca-188">Zawiera metody akcji w celu obsługi żądań GET, POST, PUT i DELETE protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="50fca-188">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
1. <span data-ttu-id="50fca-189">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="50fca-189">Build and run the app.</span></span>
1. <span data-ttu-id="50fca-190">Przejdź do `http://localhost:<port>/api/books` w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="50fca-190">Navigate to `http://localhost:<port>/api/books` in your browser.</span></span> <span data-ttu-id="50fca-191">Wyświetlane są następujące odpowiedź w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="50fca-191">The following JSON response is displayed:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="50fca-192">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="50fca-192">Next steps</span></span>

<span data-ttu-id="50fca-193">Aby uzyskać więcej informacji dotyczących tworzenia interfejsów API sieci web platformy ASP.NET Core zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="50fca-193">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
