---
page_type: sample
description: W tym samouczku przedstawiono sposób tworzenia ASP.NET Core internetowego interfejsu API przy użyciu bazy danych NoSQL MongoDB.
languages:
- csharp
products:
- dotnet-core
- aspnet-core
- vs
urlFragment: aspnetcore-webapi-mongodb
ms.openlocfilehash: 402b25f3f7c1644a52832b5c8566269773932e95
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886295"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="8af9a-102">Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i usługi MongoDB</span><span class="sxs-lookup"><span data-stu-id="8af9a-102">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="8af9a-103">Ten samouczek tworzy internetowego interfejsu API, która wykonuje operacje tworzenia, odczytu, aktualizowania lub usuwania (CRUD) [bazy danych MongoDB](https://www.mongodb.com/what-is-mongodb) bazy danych NoSQL.</span><span class="sxs-lookup"><span data-stu-id="8af9a-103">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="8af9a-104">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="8af9a-104">In this tutorial, you learn how to:</span></span>

* <span data-ttu-id="8af9a-105">Konfigurowanie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="8af9a-105">Configure MongoDB</span></span>
* <span data-ttu-id="8af9a-106">Tworzenie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="8af9a-106">Create a MongoDB database</span></span>
* <span data-ttu-id="8af9a-107">Definiowanie kolekcji usługi MongoDB i schematu</span><span class="sxs-lookup"><span data-stu-id="8af9a-107">Define a MongoDB collection and schema</span></span>
* <span data-ttu-id="8af9a-108">Wykonywanie operacji CRUD bazy danych MongoDB z internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="8af9a-108">Perform MongoDB CRUD operations from a web API</span></span>
* <span data-ttu-id="8af9a-109">Dostosowywanie serializacji JSON</span><span class="sxs-lookup"><span data-stu-id="8af9a-109">Customize JSON serialization</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8af9a-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="8af9a-110">Prerequisites</span></span>

* [<span data-ttu-id="8af9a-111">Zestaw .NET Core SDK 3,0 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="8af9a-111">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="8af9a-112">[Visual Studio 2019 Preview](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=community&ch=pre&rel=16&utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019preview) z **ASP.NET i programowaniem aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="8af9a-112">[Visual Studio 2019 Preview](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=community&ch=pre&rel=16&utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019preview) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="8af9a-113">MongoDB</span><span class="sxs-lookup"><span data-stu-id="8af9a-113">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

## <a name="configure-mongodb"></a><span data-ttu-id="8af9a-114">Konfigurowanie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="8af9a-114">Configure MongoDB</span></span>

<span data-ttu-id="8af9a-115">W przypadku korzystania z systemu Windows MongoDB jest instalowany w *C\\:\\Program Files MongoDB* domyślnie.</span><span class="sxs-lookup"><span data-stu-id="8af9a-115">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="8af9a-116">Dodaj *plik C\\: program\\Files\\MongoDB\\Serverversion_number\<>\\bin* do`Path` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8af9a-116">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="8af9a-117">Dzięki tej zmianie bazy danych MongoDB dostęp z dowolnego miejsca na komputerze deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="8af9a-117">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="8af9a-118">Użyj powłoki mongo w poniższych krokach umożliwia tworzenie bazy danych, wprowadzanie kolekcje i przechowywanie dokumentów.</span><span class="sxs-lookup"><span data-stu-id="8af9a-118">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="8af9a-119">Aby uzyskać więcej informacji na temat poleceń powłoki mongo, zobacz [Praca z powłoki mongo](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="8af9a-119">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="8af9a-120">Wybierz katalog na komputerze deweloperskim do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="8af9a-120">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="8af9a-121">Na przykład *\\C: BooksData* w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="8af9a-121">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="8af9a-122">Utwórz katalog, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="8af9a-122">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="8af9a-123">Powłoki mongo nie tworzyć nowe katalogi.</span><span class="sxs-lookup"><span data-stu-id="8af9a-123">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="8af9a-124">Otwórz powłokę wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="8af9a-124">Open a command shell.</span></span> <span data-ttu-id="8af9a-125">Uruchom następujące polecenie, aby nawiązać połączenie z bazą danych MongoDB na domyślnym porcie 27017.</span><span class="sxs-lookup"><span data-stu-id="8af9a-125">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="8af9a-126">Pamiętaj, aby zastąpić `<data_directory_path>` z katalogu, który został wybrany w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="8af9a-126">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="8af9a-127">Otwórz inne wystąpienie powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="8af9a-127">Open another command shell instance.</span></span> <span data-ttu-id="8af9a-128">Połączenia z bazą danych testu domyślną, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="8af9a-128">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="8af9a-129">Uruchom następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="8af9a-129">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="8af9a-130">Jeśli jeszcze nie istnieje, bazę danych o nazwie *BookstoreDb* zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="8af9a-130">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="8af9a-131">Jeśli baza danych istnieje, jego połączenie jest otwarte dla transakcji.</span><span class="sxs-lookup"><span data-stu-id="8af9a-131">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="8af9a-132">Utwórz `Books` kolekcji za pomocą następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="8af9a-132">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="8af9a-133">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="8af9a-133">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="8af9a-134">Zdefiniować schemat `Books` kolekcji i Wstaw dwa dokumenty, używając następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="8af9a-134">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="8af9a-135">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="8af9a-135">The following result is displayed:</span></span>

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
  > <span data-ttu-id="8af9a-136">Identyfikator przedstawiony w tym artykule nie będzie pasował do identyfikatorów podczas uruchamiania tego przykładu.</span><span class="sxs-lookup"><span data-stu-id="8af9a-136">The ID's shown in this article will not match the IDs when you run this sample.</span></span>

1. <span data-ttu-id="8af9a-137">Wyświetl dokumenty w bazie danych, używając następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="8af9a-137">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="8af9a-138">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="8af9a-138">The following result is displayed:</span></span>

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

    <span data-ttu-id="8af9a-139">Schemat dodaje wygenerowany automatycznie `_id` właściwości typu `ObjectId` dla każdego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="8af9a-139">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="8af9a-140">Baza danych jest gotowy.</span><span class="sxs-lookup"><span data-stu-id="8af9a-140">The database is ready.</span></span> <span data-ttu-id="8af9a-141">Możesz rozpocząć tworzenie interfejsu API sieci web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8af9a-141">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="8af9a-142">Utwórz projekt interfejsu API sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8af9a-142">Create the ASP.NET Core web API project</span></span>

1. <span data-ttu-id="8af9a-143">Przejdź do **pliku** > **nowe** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="8af9a-143">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="8af9a-144">Wybierz typ projektu **aplikacja sieci Web ASP.NET Core** a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="8af9a-144">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="8af9a-145">Nazwij projekt *BooksApi*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="8af9a-145">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="8af9a-146">Wybierz platformę docelową **.NET Core** i **ASP.NET Core 3,0**.</span><span class="sxs-lookup"><span data-stu-id="8af9a-146">Select the **.NET Core** target framework and **ASP.NET Core 3.0**.</span></span> <span data-ttu-id="8af9a-147">Wybierz szablon projektu **interfejsu API** i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="8af9a-147">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="8af9a-148">Odwiedź galerię [NuGet: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) , aby określić najnowszą stabilną wersję sterownika .NET dla usługi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8af9a-148">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="8af9a-149">W **Konsola Menedżera pakietów** okna, przejdź do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="8af9a-149">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="8af9a-150">Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:</span><span class="sxs-lookup"><span data-stu-id="8af9a-150">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

## <a name="add-an-entity-model"></a><span data-ttu-id="8af9a-151">Dodaj model jednostki</span><span class="sxs-lookup"><span data-stu-id="8af9a-151">Add an entity model</span></span>

1. <span data-ttu-id="8af9a-152">Dodaj *modeli* katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="8af9a-152">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="8af9a-153">Dodaj `Book` klasy *modeli* katalogu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="8af9a-153">Add a `Book` class to the *Models* directory with the following code:</span></span>

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

    <span data-ttu-id="8af9a-154">W poprzedniej klasie `Id` Właściwość:</span><span class="sxs-lookup"><span data-stu-id="8af9a-154">In the preceding class, the `Id` property:</span></span>

    * <span data-ttu-id="8af9a-155">Jest wymagany do mapowania obiektu środowiska uruchomieniowego języka wspólnego (CLR) do kolekcji MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8af9a-155">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
    * <span data-ttu-id="8af9a-156">Zawiera adnotację z [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) , aby wyznaczyć tę właściwość jako klucz podstawowy dokumentu.</span><span class="sxs-lookup"><span data-stu-id="8af9a-156">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
    * <span data-ttu-id="8af9a-157">Zawiera adnotację z [[BsonRepresentation (BsonType. objectid)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) , aby zezwolić na przekazywanie parametru jako `string` typ zamiast struktury [objectid](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) .</span><span class="sxs-lookup"><span data-stu-id="8af9a-157">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="8af9a-158">Mongo obsługuje konwersję z `string` do `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="8af9a-158">Mongo handles the conversion from `string` to `ObjectId`.</span></span>

    <span data-ttu-id="8af9a-159">Właściwość ma adnotację z atrybutem [[BsonElement].](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) `BookName`</span><span class="sxs-lookup"><span data-stu-id="8af9a-159">The `BookName` property is annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="8af9a-160">Wartość `Name` atrybutu reprezentuje nazwę właściwości w kolekcji MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8af9a-160">The attribute's value of `Name` represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="8af9a-161">Dodaj model konfiguracji</span><span class="sxs-lookup"><span data-stu-id="8af9a-161">Add a configuration model</span></span>

1. <span data-ttu-id="8af9a-162">Dodaj następujące wartości konfiguracji bazy danych do pliku *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="8af9a-162">Add the following database configuration values to *appsettings.json*:</span></span>

    ```javascript
    {
      "BookstoreDatabaseSettings": {
        "BooksCollectionName": "Books",
        "ConnectionString": "mongodb://localhost:27017",
        "DatabaseName": "BookstoreDb"
      },

    ```

1. <span data-ttu-id="8af9a-163">Dodaj plik *BookstoreDatabaseSettings.cs* do katalogu *models* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="8af9a-163">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

    ```csharp
    namespace BooksApi.Models
    {
        public class BookstoreDatabaseSettings : IBookstoreDatabaseSettings
        {
            public string BooksCollectionName { get; set; }
            public string ConnectionString { get; set; }
            public string DatabaseName { get; set; }
        }

        public interface IBookstoreDatabaseSettings
        {
            string BooksCollectionName { get; set; }
            string ConnectionString { get; set; }
            string DatabaseName { get; set; }
        }
    }
    ```

    <span data-ttu-id="8af9a-164">Powyższa `BookstoreDatabaseSettings` Klasa jest używana do przechowywania wartości `BookstoreDatabaseSettings` właściwości *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="8af9a-164">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="8af9a-165">Nazwy JSON i C# właściwości są nazywane identycznie, aby uprościć proces mapowania.</span><span class="sxs-lookup"><span data-stu-id="8af9a-165">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="8af9a-166">Dodaj następujący wyróżniony kod do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8af9a-166">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<BookstoreDatabaseSettings>(
            Configuration.GetSection(nameof(BookstoreDatabaseSettings)));

        services.AddSingleton<IBookstoreDatabaseSettings>(sp =>
            sp.GetRequiredService<IOptions<BookstoreDatabaseSettings>>().Value);

        services.AddControllers();
    }
    ```

    <span data-ttu-id="8af9a-167">W powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="8af9a-167">In the preceding code:</span></span>

    * <span data-ttu-id="8af9a-168">Wystąpienie konfiguracji, do którego są powiązane `BookstoreDatabaseSettings` sekcje pliku *appSettings. JSON* , jest zarejestrowane w kontenerze iniekcji zależności (di).</span><span class="sxs-lookup"><span data-stu-id="8af9a-168">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="8af9a-169">`BookstoreDatabaseSettings` Na przykład `ConnectionString`właściwośćobiektu jest wypełniana właściwościąwplikuappSettings.JSON.`BookstoreDatabaseSettings:ConnectionString`</span><span class="sxs-lookup"><span data-stu-id="8af9a-169">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
    * <span data-ttu-id="8af9a-170">Interfejs jest rejestrowany przy użyciu programu di z pojedynczym [okresem istnienia usługi.](xref:fundamentals/dependency-injection#service-lifetimes) `IBookstoreDatabaseSettings`</span><span class="sxs-lookup"><span data-stu-id="8af9a-170">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="8af9a-171">Po dowstrzykiwaniu wystąpienie interfejsu jest rozpoznawane `BookstoreDatabaseSettings` jako obiekt.</span><span class="sxs-lookup"><span data-stu-id="8af9a-171">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="8af9a-172">Dodaj następujący kod na początku *Startup.cs* , aby rozwiązać `BookstoreDatabaseSettings` odwołania i: `IBookstoreDatabaseSettings`</span><span class="sxs-lookup"><span data-stu-id="8af9a-172">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

    ```csharp
    using BooksApi.Models;
    ```

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="8af9a-173">Dodawanie usługi operacji CRUD</span><span class="sxs-lookup"><span data-stu-id="8af9a-173">Add a CRUD operations service</span></span>

1. <span data-ttu-id="8af9a-174">Dodaj *usług* katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="8af9a-174">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="8af9a-175">Dodaj `BookService` klasy *usług* katalogu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="8af9a-175">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    ```csharp
    using BooksApi.Models;
    using MongoDB.Driver;
    using System.Collections.Generic;
    using System.Linq;

    namespace BooksApi.Services
    {
        public class BookService
        {
            private readonly IMongoCollection<Book> _books;

            public BookService(IBookstoreDatabaseSettings settings)
            {
                var client = new MongoClient(settings.ConnectionString);
                var database = client.GetDatabase(settings.DatabaseName);

                _books = database.GetCollection<Book>(settings.BooksCollectionName);
            }

            public List<Book> Get() =>
                _books.Find(book => true).ToList();

            public Book Get(string id) =>
                _books.Find<Book>(book => book.Id == id).FirstOrDefault();

            public Book Create(Book book)
            {
                _books.InsertOne(book);
                return book;
            }

            public void Update(string id, Book bookIn) =>
                _books.ReplaceOne(book => book.Id == id, bookIn);

            public void Remove(Book bookIn) =>
                _books.DeleteOne(book => book.Id == bookIn.Id);

            public void Remove(string id) =>
                _books.DeleteOne(book => book.Id == id);
        }
    }
    ```

    <span data-ttu-id="8af9a-176">W poprzednim kodzie `IBookstoreDatabaseSettings` wystąpienie jest pobierane z funkcji di przez iniekcję konstruktora.</span><span class="sxs-lookup"><span data-stu-id="8af9a-176">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="8af9a-177">Ta technika zapewnia dostęp do wartości konfiguracyjnych *appSettings. JSON* , które zostały dodane w sekcji [Dodawanie modelu konfiguracji](#add-a-configuration-model) .</span><span class="sxs-lookup"><span data-stu-id="8af9a-177">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="8af9a-178">Dodaj następujący wyróżniony kod do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8af9a-178">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<BookstoreDatabaseSettings>(
            Configuration.GetSection(nameof(BookstoreDatabaseSettings)));

        services.AddSingleton<IBookstoreDatabaseSettings>(sp =>
            sp.GetRequiredService<IOptions<BookstoreDatabaseSettings>>().Value);

        services.AddSingleton<BookService>();

        services.AddControllers();
    }
    ```

    <span data-ttu-id="8af9a-179">W poprzednim kodzie `BookService` Klasa jest zarejestrowana przy użyciu funkcji di, aby obsługiwać iniekcję konstruktora w klasach zużywających.</span><span class="sxs-lookup"><span data-stu-id="8af9a-179">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="8af9a-180">Okres istnienia usługi pojedynczej jest najbardziej `BookService` odpowiedni, ponieważ pobiera bezpośrednią zależność od. `MongoClient`</span><span class="sxs-lookup"><span data-stu-id="8af9a-180">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="8af9a-181">Zgodnie z oficjalnymi wskazówkami dotyczącymi `MongoClient` [ponownego użycia klienta Mongo](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)należy zarejestrować się w programie di z pojedynczym okresem istnienia usługi.</span><span class="sxs-lookup"><span data-stu-id="8af9a-181">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="8af9a-182">Dodaj następujący kod na początku *Startup.cs* , aby rozwiązać `BookService` odwołanie:</span><span class="sxs-lookup"><span data-stu-id="8af9a-182">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>


    ```csharp
    using BooksApi.Services;
    ```

<span data-ttu-id="8af9a-183">`BookService` Klasa używa następujących `MongoDB.Driver` elementy członkowskie do wykonywania operacji CRUD w odniesieniu do bazy danych:</span><span class="sxs-lookup"><span data-stu-id="8af9a-183">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="8af9a-184">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Odczytuje wystąpienie serwera na potrzeby wykonywania operacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8af9a-184">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="8af9a-185">Konstruktor obiektu tej klasy znajduje się ciąg połączenia bazy danych MongoDB:</span><span class="sxs-lookup"><span data-stu-id="8af9a-185">The constructor of this class is provided the MongoDB connection string:</span></span>

    ```csharp
    public BookService(IBookstoreDatabaseSettings settings)
    {
        var client = new MongoClient(settings.ConnectionString);
        var database = client.GetDatabase(settings.DatabaseName);

        _books = database.GetCollection<Book>(settings.BooksCollectionName);
    }
    ```

* <span data-ttu-id="8af9a-186">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Reprezentuje bazę danych Mongo na potrzeby wykonywania operacji.</span><span class="sxs-lookup"><span data-stu-id="8af9a-186">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="8af9a-187">W tym samouczku do uzyskiwania dostępu do danych w określonej kolekcji jest stosowana ogólna Metoda [getcollection\<TDocument > (Collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) .</span><span class="sxs-lookup"><span data-stu-id="8af9a-187">This tutorial uses the generic [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="8af9a-188">Wykonaj operacje CRUD w odniesieniu do kolekcji po wywołaniu tej metody.</span><span class="sxs-lookup"><span data-stu-id="8af9a-188">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="8af9a-189">W `GetCollection<TDocument>(collection)` wywołanie metody:</span><span class="sxs-lookup"><span data-stu-id="8af9a-189">In the `GetCollection<TDocument>(collection)` method call:</span></span>
  * <span data-ttu-id="8af9a-190">`collection` reprezentuje nazwę kolekcji.</span><span class="sxs-lookup"><span data-stu-id="8af9a-190">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="8af9a-191">`TDocument` reprezentuje typ obiektu CLR, które są przechowywane w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="8af9a-191">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="8af9a-192">`GetCollection<TDocument>(collection)`zwraca obiekt [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) reprezentujący kolekcję.</span><span class="sxs-lookup"><span data-stu-id="8af9a-192">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="8af9a-193">W tym samouczku następujące metody są wywoływane w kolekcji:</span><span class="sxs-lookup"><span data-stu-id="8af9a-193">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="8af9a-194">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Usuwa pojedynczy dokument pasujący do podanych kryteriów wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="8af9a-194">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="8af9a-195">[Znajdź\<TDocument >](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; zwraca wszystkie dokumenty w kolekcji pasujące do podanych kryteriów wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="8af9a-195">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="8af9a-196">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Wstawia podany obiekt jako nowy dokument w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="8af9a-196">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="8af9a-197">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Zamienia pojedynczy dokument pasujący do podanych kryteriów wyszukiwania z podanym obiektem.</span><span class="sxs-lookup"><span data-stu-id="8af9a-197">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="8af9a-198">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="8af9a-198">Add a controller</span></span>

<span data-ttu-id="8af9a-199">Dodaj `BooksController` klasy *kontrolerów* katalogu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="8af9a-199">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

```csharp
using BooksApi.Models;
using BooksApi.Services;
using Microsoft.AspNetCore.Mvc;
using System.Collections.Generic;

namespace BooksApi.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class BooksController : ControllerBase
    {
        private readonly BookService _bookService;

        public BooksController(BookService bookService)
        {
            _bookService = bookService;
        }

        [HttpGet]
        public ActionResult<List<Book>> Get() =>
            _bookService.Get();

        [HttpGet("{id:length(24)}", Name = "GetBook")]
        public ActionResult<Book> Get(string id)
        {
            var book = _bookService.Get(id);

            if (book == null)
            {
                return NotFound();
            }

            return book;
        }

        [HttpPost]
        public ActionResult<Book> Create(Book book)
        {
            _bookService.Create(book);

            return CreatedAtRoute("GetBook", new { id = book.Id.ToString() }, book);
        }

        [HttpPut("{id:length(24)}")]
        public IActionResult Update(string id, Book bookIn)
        {
            var book = _bookService.Get(id);

            if (book == null)
            {
                return NotFound();
            }

            _bookService.Update(id, bookIn);

            return NoContent();
        }

        [HttpDelete("{id:length(24)}")]
        public IActionResult Delete(string id)
        {
            var book = _bookService.Get(id);

            if (book == null)
            {
                return NotFound();
            }

            _bookService.Remove(book.Id);

            return NoContent();
        }
    }
}
```

<span data-ttu-id="8af9a-200">Poprzedni kontroler internetowego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="8af9a-200">The preceding web API controller:</span></span>

* <span data-ttu-id="8af9a-201">Używa `BookService` klasy w celu wykonywania operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="8af9a-201">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="8af9a-202">Zawiera metody akcji w celu obsługi żądań GET, POST, PUT i DELETE protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="8af9a-202">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="8af9a-203">Wywołuje <xref:System.Web.Http.ApiController.CreatedAtRoute*> metodę akcji `Create` , aby zwrócić odpowiedź [http 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) .</span><span class="sxs-lookup"><span data-stu-id="8af9a-203">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="8af9a-204">Kod stanu 201 jest standardową odpowiedzią dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="8af9a-204">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="8af9a-205">`CreatedAtRoute`dodaje `Location` również nagłówek do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="8af9a-205">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="8af9a-206">`Location` Nagłówek określa identyfikator URI nowo utworzonej książki.</span><span class="sxs-lookup"><span data-stu-id="8af9a-206">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="8af9a-207">Testowanie interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="8af9a-207">Test the web API</span></span>

1. <span data-ttu-id="8af9a-208">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="8af9a-208">Build and run the app.</span></span>

1. <span data-ttu-id="8af9a-209">Przejdź do `http://localhost:<port>/api/books` w celu przetestowania metody `Get` akcji bez parametrów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="8af9a-209">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="8af9a-210">Wyświetlane są następujące odpowiedź w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="8af9a-210">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="8af9a-211">Przejdź do `http://localhost:<port>/api/books/{id here}` , aby przetestować przeciążoną `Get` metodę działania kontrolera.</span><span class="sxs-lookup"><span data-stu-id="8af9a-211">Navigate to `http://localhost:<port>/api/books/{id here}` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="8af9a-212">Wyświetlane są następujące odpowiedź w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="8af9a-212">The following JSON response is displayed:</span></span>

    ```json
    {
      "id":"{ID}",
      "bookName":"Clean Code",
      "price":43.15,
      "category":"Computers",
      "author":"Robert C. Martin"
    }
    ```

## <a name="configure-json-serialization-options"></a><span data-ttu-id="8af9a-213">Konfigurowanie opcji serializacji JSON</span><span class="sxs-lookup"><span data-stu-id="8af9a-213">Configure JSON serialization options</span></span>

<span data-ttu-id="8af9a-214">Istnieją dwa szczegóły dotyczące odpowiedzi JSON zwróconych w sekcji [testowanie interfejsu API sieci Web](#test-the-web-api) :</span><span class="sxs-lookup"><span data-stu-id="8af9a-214">There are two details to change about the JSON responses returned in the [Test the web API](#test-the-web-api) section:</span></span>

* <span data-ttu-id="8af9a-215">Nazwy właściwości "default notacji CamelCase wielkość liter należy zmienić tak, aby pasowały do wielkości liter w języku Pascal nazw właściwości obiektu CLR.</span><span class="sxs-lookup"><span data-stu-id="8af9a-215">The property names' default camel casing should be changed to match the Pascal casing of the CLR object's property names.</span></span>
* <span data-ttu-id="8af9a-216">Właściwość powinna zostać zwrócona jako `Name`. `bookName`</span><span class="sxs-lookup"><span data-stu-id="8af9a-216">The `bookName` property should be returned as `Name`.</span></span>

<span data-ttu-id="8af9a-217">Aby spełnić powyższe wymagania, należy wprowadzić następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="8af9a-217">To satisfy the preceding requirements, make the following changes:</span></span>

1. <span data-ttu-id="8af9a-218">JSON.NET został usunięty z ASP.NET udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="8af9a-218">JSON.NET has been removed from ASP.NET shared framework.</span></span> <span data-ttu-id="8af9a-219">Dodaj odwołanie do pakietu do [Microsoft. AspNetCore. MVC. NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson).</span><span class="sxs-lookup"><span data-stu-id="8af9a-219">Add a package reference to [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson).</span></span>

1. <span data-ttu-id="8af9a-220">W `Startup.ConfigureServices`programie łańcuchować następujący wyróżniony kod w `AddMvc` wywołaniu metody:</span><span class="sxs-lookup"><span data-stu-id="8af9a-220">In `Startup.ConfigureServices`, chain the following highlighted code on to the `AddMvc` method call:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<BookstoreDatabaseSettings>(
            Configuration.GetSection(nameof(BookstoreDatabaseSettings)));

        services.AddSingleton<IBookstoreDatabaseSettings>(sp =>
            sp.GetRequiredService<IOptions<BookstoreDatabaseSettings>>().Value);

        services.AddSingleton<BookService>();

        services.AddControllers()
            .AddNewtonsoftJson(options => options.UseMemberCasing());
    }
    ```

    <span data-ttu-id="8af9a-221">W przypadku poprzedniej zmiany nazwy właściwości w serializowanej odpowiedzi JSON interfejsu API sieci Web pasują do odpowiednich nazw właściwości w typie obiektu CLR.</span><span class="sxs-lookup"><span data-stu-id="8af9a-221">With the preceding change, property names in the web API's serialized JSON response match their corresponding property names in the CLR object type.</span></span> <span data-ttu-id="8af9a-222">Na przykład, `Book` Serializacja `Author` właściwości klasy jako `Author`.</span><span class="sxs-lookup"><span data-stu-id="8af9a-222">For example, the `Book` class's `Author` property serializes as `Author`.</span></span>

1. <span data-ttu-id="8af9a-223">W *modelach/książka. cs*Dodaj adnotację `BookName` do właściwości z następującym atrybutem [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) :</span><span class="sxs-lookup"><span data-stu-id="8af9a-223">In *Models/Book.cs*, annotate the `BookName` property with the following [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) attribute:</span></span>

    ```csharp
    [BsonElement("Name")]
    [JsonProperty("Name")]
    public string BookName { get; set; }
    ```

    <span data-ttu-id="8af9a-224">`[JsonProperty]` WartośćatrybutureprezentujenazwęwłaściwościwserializowanejodpowiedziJSON`Name` interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8af9a-224">The `[JsonProperty]` attribute's value of `Name` represents the property name in the web API's serialized JSON response.</span></span>

1. <span data-ttu-id="8af9a-225">Dodaj następujący kod na górze *modeli/książek. cs* , aby rozwiązać `[JsonProperty]` odwołanie do atrybutu:</span><span class="sxs-lookup"><span data-stu-id="8af9a-225">Add the following code to the top of *Models/Book.cs* to resolve the `[JsonProperty]` attribute reference:</span></span>

    ```csharp
    using Newtonsoft.Json;
    ```

1. <span data-ttu-id="8af9a-226">Powtórz kroki zdefiniowane w sekcji [testowanie interfejsu API sieci Web](#test-the-web-api) .</span><span class="sxs-lookup"><span data-stu-id="8af9a-226">Repeat the steps defined in the [Test the web API](#test-the-web-api) section.</span></span> <span data-ttu-id="8af9a-227">Zwróć uwagę na różnice w nazwach właściwości JSON.</span><span class="sxs-lookup"><span data-stu-id="8af9a-227">Notice the difference in JSON property names.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8af9a-228">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="8af9a-228">Next steps</span></span>

<span data-ttu-id="8af9a-229">Aby uzyskać więcej informacji dotyczących tworzenia interfejsów API sieci web platformy ASP.NET Core zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="8af9a-229">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="8af9a-230">Wersja tego artykułu usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="8af9a-230">YouTube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* [<span data-ttu-id="8af9a-231">Tworzenie internetowych interfejsów API za pomocą ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8af9a-231">Create web APIs with ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/web-api/index?view=aspnetcore-3.0)
