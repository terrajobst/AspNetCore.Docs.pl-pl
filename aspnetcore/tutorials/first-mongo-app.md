---
title: Tworzenie interfejsów API sieci web za pomocą platformy ASP.NET Core i bazy danych MongoDB
author: prkhandelwal
description: W tym samouczku przedstawiono sposób tworzenia sieci web platformy ASP.NET Core interfejsu API przy użyciu bazy danych NoSQL bazy danych MongoDB.
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 11/29/2018
uid: tutorials/first-mongo-app
ms.openlocfilehash: bd9a36c5eb06542c820e71e937b8da10f735a0f8
ms.sourcegitcommit: 68a3081dd175d6518d1bfa31b4712bd8a2dd3864
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53577841"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="71e52-103">Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i usługi MongoDB</span><span class="sxs-lookup"><span data-stu-id="71e52-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="71e52-104">Przez [Pratik Khandelwal](https://twitter.com/K2Prk) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="71e52-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="71e52-105">Ten samouczek tworzy internetowego interfejsu API, która wykonuje operacje tworzenia, odczytu, aktualizowania lub usuwania (CRUD) [bazy danych MongoDB](https://www.mongodb.com/what-is-mongodb) bazy danych NoSQL.</span><span class="sxs-lookup"><span data-stu-id="71e52-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="71e52-106">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="71e52-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="71e52-107">Konfigurowanie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="71e52-107">Configure MongoDB</span></span>
> * <span data-ttu-id="71e52-108">Tworzenie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="71e52-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="71e52-109">Definiowanie kolekcji usługi MongoDB i schematu</span><span class="sxs-lookup"><span data-stu-id="71e52-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="71e52-110">Wykonywanie operacji CRUD bazy danych MongoDB z internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="71e52-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="71e52-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="71e52-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="71e52-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="71e52-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71e52-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71e52-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="71e52-114">.NET core SDK 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="71e52-114">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="71e52-115">[Visual Studio 2017 w wersji 15.9 lub nowszej](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia</span><span class="sxs-lookup"><span data-stu-id="71e52-115">[Visual Studio 2017 version 15.9 or later](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="71e52-116">MongoDB</span><span class="sxs-lookup"><span data-stu-id="71e52-116">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71e52-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71e52-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="71e52-118">.NET core SDK 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="71e52-118">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="71e52-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71e52-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="71e52-120">Środowisko C# dla programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71e52-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="71e52-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="71e52-121">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71e52-122">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="71e52-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="71e52-123">.NET core SDK 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="71e52-123">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="71e52-124">Program Visual Studio dla komputerów Mac w wersji 7,7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="71e52-124">Visual Studio for Mac version 7.7 or later</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="71e52-125">MongoDB</span><span class="sxs-lookup"><span data-stu-id="71e52-125">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="71e52-126">Konfigurowanie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="71e52-126">Configure MongoDB</span></span>

<span data-ttu-id="71e52-127">Jeśli przy użyciu Windows, bazy danych MongoDB jest zainstalowana w *C:\Program Files\MongoDB* domyślnie.</span><span class="sxs-lookup"><span data-stu-id="71e52-127">If using Windows, MongoDB is installed at *C:\Program Files\MongoDB* by default.</span></span> <span data-ttu-id="71e52-128">Dodaj *C:\Program Files\MongoDB\Server\<numer_wersji > \bin* do `Path` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="71e52-128">Add *C:\Program Files\MongoDB\Server\<version_number>\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="71e52-129">Dzięki tej zmianie bazy danych MongoDB dostęp z dowolnego miejsca na komputerze deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="71e52-129">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="71e52-130">Użyj powłoki mongo w poniższych krokach umożliwia tworzenie bazy danych, wprowadzanie kolekcje i przechowywanie dokumentów.</span><span class="sxs-lookup"><span data-stu-id="71e52-130">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="71e52-131">Aby uzyskać więcej informacji na temat poleceń powłoki mongo, zobacz [Praca z powłoki mongo](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="71e52-131">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="71e52-132">Wybierz katalog na komputerze deweloperskim do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="71e52-132">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="71e52-133">Na przykład *C:\BooksData* na Windows.</span><span class="sxs-lookup"><span data-stu-id="71e52-133">For example, *C:\BooksData* on Windows.</span></span> <span data-ttu-id="71e52-134">Utwórz katalog, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="71e52-134">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="71e52-135">Powłoki mongo nie tworzyć nowe katalogi.</span><span class="sxs-lookup"><span data-stu-id="71e52-135">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="71e52-136">Otwórz powłokę wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="71e52-136">Open a command shell.</span></span> <span data-ttu-id="71e52-137">Uruchom następujące polecenie, aby nawiązać połączenie z bazą danych MongoDB na domyślnym porcie 27017.</span><span class="sxs-lookup"><span data-stu-id="71e52-137">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="71e52-138">Pamiętaj, aby zastąpić `<data_directory_path>` z katalogu, który został wybrany w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="71e52-138">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="71e52-139">Otwórz inne wystąpienie powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="71e52-139">Open another command shell instance.</span></span> <span data-ttu-id="71e52-140">Połączenia z bazą danych testu domyślną, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="71e52-140">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="71e52-141">Uruchom następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="71e52-141">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="71e52-142">Jeśli jeszcze nie istnieje, bazę danych o nazwie *BookstoreDb* zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="71e52-142">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="71e52-143">Jeśli baza danych istnieje, jego połączenie jest otwarte dla transakcji.</span><span class="sxs-lookup"><span data-stu-id="71e52-143">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="71e52-144">Utwórz `Books` kolekcji za pomocą następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="71e52-144">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="71e52-145">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="71e52-145">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="71e52-146">Zdefiniować schemat `Books` kolekcji i Wstaw dwa dokumenty, używając następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="71e52-146">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="71e52-147">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="71e52-147">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="71e52-148">Wyświetl dokumenty w bazie danych, używając następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="71e52-148">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="71e52-149">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="71e52-149">The following result is displayed:</span></span>

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

    <span data-ttu-id="71e52-150">Schemat dodaje wygenerowany automatycznie `_id` właściwości typu `ObjectId` dla każdego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="71e52-150">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="71e52-151">Baza danych jest gotowy.</span><span class="sxs-lookup"><span data-stu-id="71e52-151">The database is ready.</span></span> <span data-ttu-id="71e52-152">Możesz rozpocząć tworzenie interfejsu API sieci web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="71e52-152">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="71e52-153">Utwórz projekt interfejsu API sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="71e52-153">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="71e52-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="71e52-154">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="71e52-155">Przejdź do **pliku** > **nowe** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="71e52-155">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="71e52-156">Wybierz **aplikacji sieci Web programu ASP.NET Core**, nadaj projektowi nazwę *BooksApi*i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="71e52-156">Select **ASP.NET Core Web Application**, name the project *BooksApi*, and click **OK**.</span></span>
1. <span data-ttu-id="71e52-157">Wybierz **platformy .NET Core** platformy docelowej i **platformy ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="71e52-157">Select the **.NET Core** target framework and **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="71e52-158">Wybierz **API** projektu szablonu, a następnie kliknij przycisk **OK**:</span><span class="sxs-lookup"><span data-stu-id="71e52-158">Select the **API** project template, and click **OK**:</span></span>
1. <span data-ttu-id="71e52-159">W **Konsola Menedżera pakietów** okna, przejdź do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="71e52-159">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="71e52-160">Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:</span><span class="sxs-lookup"><span data-stu-id="71e52-160">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version 2.7.2
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="71e52-161">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="71e52-161">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="71e52-162">Uruchom następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="71e52-162">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="71e52-163">Nowy projekt interfejsu API sieci web platformy ASP.NET Core, na przeznaczonych dla platformy .NET Core jest wygenerowany i otworzyć w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="71e52-163">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="71e52-164">Kliknij przycisk **tak** podczas *"BooksApi" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?*  zostanie wyświetlone powiadomienie.</span><span class="sxs-lookup"><span data-stu-id="71e52-164">Click **Yes** when the *Required assets to build and debug are missing from 'BooksApi'. Add them?* notification appears.</span></span>
1. <span data-ttu-id="71e52-165">Otwórz **zintegrowany Terminal** i przejdź do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="71e52-165">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="71e52-166">Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:</span><span class="sxs-lookup"><span data-stu-id="71e52-166">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v 2.7.2
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="71e52-167">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="71e52-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="71e52-168">Przejdź do **pliku** > **nowe rozwiązanie** > **platformy .NET Core** > **aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="71e52-168">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="71e52-169">Wybierz **internetowego interfejsu API platformy ASP.NET Core** C# projektu szablonu, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="71e52-169">Select the **ASP.NET Core Web API** C# project template, and click **Next**.</span></span>
1. <span data-ttu-id="71e52-170">Wybierz **platformy .NET Core 2.2** z **platformę docelową** listy rozwijanej, a następnie kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="71e52-170">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and click **Next**.</span></span>
1. <span data-ttu-id="71e52-171">Wprowadź *BooksApi* dla **Nazwa projektu**i kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="71e52-171">Enter *BooksApi* for the **Project Name**, and click **Create**.</span></span>
1. <span data-ttu-id="71e52-172">W **rozwiązania** konsoli kliknij prawym przyciskiem myszy projekt **zależności** a następnie wybierz węzeł **Dodawanie pakietów**.</span><span class="sxs-lookup"><span data-stu-id="71e52-172">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="71e52-173">Wprowadź *MongoDB.Driver* w polu wyszukiwania, wybierz *MongoDB.Driver* pakietu, a następnie kliknij przycisk **Dodaj pakiet**.</span><span class="sxs-lookup"><span data-stu-id="71e52-173">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and click **Add Package**.</span></span>
1. <span data-ttu-id="71e52-174">Kliknij przycisk **Akceptuj** znajdujący się w **akceptacja licencji** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="71e52-174">Click the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-a-model"></a><span data-ttu-id="71e52-175">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="71e52-175">Add a model</span></span>

1. <span data-ttu-id="71e52-176">Dodaj *modeli* katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="71e52-176">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="71e52-177">Dodaj `Book` klasy *modeli* katalogu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="71e52-177">Add a `Book` class to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

<span data-ttu-id="71e52-178">W klasie poprzedniego `Id` właściwość jest wymagana do mapowania obiektu środowiska uruchomieniowego języka wspólnego (CLR) do kolekcji usługi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="71e52-178">In the preceding class, the `Id` property is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span> <span data-ttu-id="71e52-179">Inne właściwości w klasie są ozdobione `[BsonElement]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="71e52-179">Other properties in the class are decorated with the `[BsonElement]` attribute.</span></span> <span data-ttu-id="71e52-180">Wartość ten atrybut reprezentuje nazwę właściwości w kolekcji usługi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="71e52-180">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-crud-operations-class"></a><span data-ttu-id="71e52-181">Dodaj klasę operacje CRUD</span><span class="sxs-lookup"><span data-stu-id="71e52-181">Add a CRUD operations class</span></span>

1. <span data-ttu-id="71e52-182">Dodaj *usług* katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="71e52-182">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="71e52-183">Dodaj `BookService` klasy *usług* katalogu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="71e52-183">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

1. <span data-ttu-id="71e52-184">Dodaj parametry połączenia bazy danych MongoDB do *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="71e52-184">Add the MongoDB connection string to *appsettings.json*:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-4)]

    <span data-ttu-id="71e52-185">Poprzedni `BookstoreDb` dostęp do właściwości w `BookService` konstruktora klasy.</span><span class="sxs-lookup"><span data-stu-id="71e52-185">The preceding `BookstoreDb` property is accessed in the `BookService` class constructor.</span></span>

1. <span data-ttu-id="71e52-186">W `Startup.ConfigureServices`, zarejestruj `BookService` klasy przy użyciu systemu wstrzykiwanie zależności:</span><span class="sxs-lookup"><span data-stu-id="71e52-186">In `Startup.ConfigureServices`, register the `BookService` class with the Dependency Injection system:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

    <span data-ttu-id="71e52-187">Poprzedni rejestracji usługi jest niezbędne do obsługi iniekcji konstruktora w korzystających z tych klas.</span><span class="sxs-lookup"><span data-stu-id="71e52-187">The preceding service registration is necessary to support constructor injection in consuming classes.</span></span>

<span data-ttu-id="71e52-188">`BookService` Klasa używa następujących `MongoDB.Driver` elementy członkowskie do wykonywania operacji CRUD w odniesieniu do bazy danych:</span><span class="sxs-lookup"><span data-stu-id="71e52-188">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="71e52-189">`MongoClient` &ndash; Odczytuje wystąpienie serwera do wykonywania operacji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="71e52-189">`MongoClient` &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="71e52-190">Konstruktor obiektu tej klasy znajduje się ciąg połączenia bazy danych MongoDB:</span><span class="sxs-lookup"><span data-stu-id="71e52-190">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="71e52-191">`IMongoDatabase` &ndash; Reprezentuje bazą danych Mongo do wykonywania operacji.</span><span class="sxs-lookup"><span data-stu-id="71e52-191">`IMongoDatabase` &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="71e52-192">W tym samouczku korzysta z ogólnego `GetCollection<T>(collection)` metoda w interfejsie, aby uzyskać dostęp do danych w określonej kolekcji.</span><span class="sxs-lookup"><span data-stu-id="71e52-192">This tutorial uses the generic `GetCollection<T>(collection)` method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="71e52-193">Po ta metoda jest wywoływana, można wykonać operacji CRUD względem kolekcji.</span><span class="sxs-lookup"><span data-stu-id="71e52-193">CRUD operations can be performed against the collection after this method is called.</span></span> <span data-ttu-id="71e52-194">W `GetCollection<T>(collection)` wywołanie metody:</span><span class="sxs-lookup"><span data-stu-id="71e52-194">In the `GetCollection<T>(collection)` method call:</span></span>
  * <span data-ttu-id="71e52-195">`collection` reprezentuje nazwę kolekcji.</span><span class="sxs-lookup"><span data-stu-id="71e52-195">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="71e52-196">`T` reprezentuje typ obiektu CLR, które są przechowywane w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="71e52-196">`T` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="71e52-197">`GetCollection<T>(collection)` Zwraca `MongoCollection` obiekt reprezentujący kolekcję.</span><span class="sxs-lookup"><span data-stu-id="71e52-197">`GetCollection<T>(collection)` returns a `MongoCollection` object representing the collection.</span></span> <span data-ttu-id="71e52-198">W tym samouczku następujące metody są wywoływane w kolekcji:</span><span class="sxs-lookup"><span data-stu-id="71e52-198">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="71e52-199">`Find<T>` &ndash; Zwraca wszystkie dokumenty w kolekcji, zgodne z kryteriami wyszukiwania podana.</span><span class="sxs-lookup"><span data-stu-id="71e52-199">`Find<T>` &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="71e52-200">`InsertOne` &ndash; Wstawia podany obiekt jako nowy dokument w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="71e52-200">`InsertOne` &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="71e52-201">`ReplaceOne` &ndash; Zastępuje jednolitego dokumentu, które są zgodne z kryteriami wyszukiwania podanej za pomocą udostępnionego obiektu.</span><span class="sxs-lookup"><span data-stu-id="71e52-201">`ReplaceOne` &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>
* <span data-ttu-id="71e52-202">`DeleteOne` &ndash; Usuwa pojedynczy dokument zgodne z kryteriami wyszukiwania podana.</span><span class="sxs-lookup"><span data-stu-id="71e52-202">`DeleteOne` &ndash; Deletes a single document matching the provided search criteria.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="71e52-203">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="71e52-203">Add a controller</span></span>

1. <span data-ttu-id="71e52-204">Dodaj `BooksController` klasy *kontrolerów* katalogu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="71e52-204">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

    <span data-ttu-id="71e52-205">Poprzedni kontroler internetowego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="71e52-205">The preceding web API controller:</span></span>

    * <span data-ttu-id="71e52-206">Używa `BookService` klasy w celu wykonywania operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="71e52-206">Uses the `BookService` class to perform CRUD operations.</span></span>
    * <span data-ttu-id="71e52-207">Zawiera metody akcji w celu obsługi żądań GET, POST, PUT i DELETE protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="71e52-207">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
1. <span data-ttu-id="71e52-208">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="71e52-208">Build and run the app.</span></span>
1. <span data-ttu-id="71e52-209">Przejdź do `http://localhost:<port>/api/books` w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="71e52-209">Navigate to `http://localhost:<port>/api/books` in your browser.</span></span> <span data-ttu-id="71e52-210">Wyświetlane są następujące odpowiedź w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="71e52-210">The following JSON response is displayed:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="71e52-211">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="71e52-211">Next steps</span></span>

<span data-ttu-id="71e52-212">Aby uzyskać więcej informacji dotyczących tworzenia interfejsów API sieci web platformy ASP.NET Core zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="71e52-212">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
