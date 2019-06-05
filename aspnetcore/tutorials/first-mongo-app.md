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
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="fcadc-103">Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i usługi MongoDB</span><span class="sxs-lookup"><span data-stu-id="fcadc-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="fcadc-104">Przez [Pratik Khandelwal](https://twitter.com/K2Prk) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="fcadc-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="fcadc-105">Ten samouczek tworzy internetowego interfejsu API, która wykonuje operacje tworzenia, odczytu, aktualizowania lub usuwania (CRUD) [bazy danych MongoDB](https://www.mongodb.com/what-is-mongodb) bazy danych NoSQL.</span><span class="sxs-lookup"><span data-stu-id="fcadc-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="fcadc-106">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="fcadc-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fcadc-107">Konfigurowanie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="fcadc-107">Configure MongoDB</span></span>
> * <span data-ttu-id="fcadc-108">Tworzenie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="fcadc-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="fcadc-109">Definiowanie kolekcji usługi MongoDB i schematu</span><span class="sxs-lookup"><span data-stu-id="fcadc-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="fcadc-110">Wykonywanie operacji CRUD bazy danych MongoDB z internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="fcadc-110">Perform MongoDB CRUD operations from a web API</span></span>

<span data-ttu-id="fcadc-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fcadc-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fcadc-112">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="fcadc-112">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fcadc-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fcadc-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="fcadc-114">.NET core SDK 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="fcadc-114">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="fcadc-115">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia</span><span class="sxs-lookup"><span data-stu-id="fcadc-115">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="fcadc-116">MongoDB</span><span class="sxs-lookup"><span data-stu-id="fcadc-116">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fcadc-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fcadc-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="fcadc-118">.NET core SDK 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="fcadc-118">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="fcadc-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fcadc-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="fcadc-120">Środowisko C# dla programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fcadc-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="fcadc-121">MongoDB</span><span class="sxs-lookup"><span data-stu-id="fcadc-121">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fcadc-122">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fcadc-122">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="fcadc-123">.NET core SDK 2,2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="fcadc-123">.NET Core SDK 2.2 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="fcadc-124">Program Visual Studio dla komputerów Mac w wersji 7,7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="fcadc-124">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="fcadc-125">MongoDB</span><span class="sxs-lookup"><span data-stu-id="fcadc-125">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="fcadc-126">Konfigurowanie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="fcadc-126">Configure MongoDB</span></span>

<span data-ttu-id="fcadc-127">Jeśli przy użyciu Windows, bazy danych MongoDB jest zainstalowana w *C:\\Program Files\\bazy danych MongoDB* domyślnie.</span><span class="sxs-lookup"><span data-stu-id="fcadc-127">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="fcadc-128">Dodaj *C:\\Program Files\\bazy danych MongoDB\\serwera\\\<numer_wersji >\\bin* do `Path` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="fcadc-128">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="fcadc-129">Dzięki tej zmianie bazy danych MongoDB dostęp z dowolnego miejsca na komputerze deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="fcadc-129">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="fcadc-130">Użyj powłoki mongo w poniższych krokach umożliwia tworzenie bazy danych, wprowadzanie kolekcje i przechowywanie dokumentów.</span><span class="sxs-lookup"><span data-stu-id="fcadc-130">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="fcadc-131">Aby uzyskać więcej informacji na temat poleceń powłoki mongo, zobacz [Praca z powłoki mongo](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="fcadc-131">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="fcadc-132">Wybierz katalog na komputerze deweloperskim do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="fcadc-132">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="fcadc-133">Na przykład *C:\\BooksData* na Windows.</span><span class="sxs-lookup"><span data-stu-id="fcadc-133">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="fcadc-134">Utwórz katalog, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="fcadc-134">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="fcadc-135">Powłoki mongo nie tworzyć nowe katalogi.</span><span class="sxs-lookup"><span data-stu-id="fcadc-135">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="fcadc-136">Otwórz powłokę wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="fcadc-136">Open a command shell.</span></span> <span data-ttu-id="fcadc-137">Uruchom następujące polecenie, aby nawiązać połączenie z bazą danych MongoDB na domyślnym porcie 27017.</span><span class="sxs-lookup"><span data-stu-id="fcadc-137">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="fcadc-138">Pamiętaj, aby zastąpić `<data_directory_path>` z katalogu, który został wybrany w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="fcadc-138">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

    ```console
    mongod --dbpath <data_directory_path>
    ```

1. <span data-ttu-id="fcadc-139">Otwórz inne wystąpienie powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="fcadc-139">Open another command shell instance.</span></span> <span data-ttu-id="fcadc-140">Połączenia z bazą danych testu domyślną, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="fcadc-140">Connect to the default test database by running the following command:</span></span>

    ```console
    mongo
    ```

1. <span data-ttu-id="fcadc-141">Uruchom następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="fcadc-141">Run the following in a command shell:</span></span>

    ```console
    use BookstoreDb
    ```

    <span data-ttu-id="fcadc-142">Jeśli jeszcze nie istnieje, bazę danych o nazwie *BookstoreDb* zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="fcadc-142">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="fcadc-143">Jeśli baza danych istnieje, jego połączenie jest otwarte dla transakcji.</span><span class="sxs-lookup"><span data-stu-id="fcadc-143">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="fcadc-144">Utwórz `Books` kolekcji za pomocą następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="fcadc-144">Create a `Books` collection using following command:</span></span>

    ```console
    db.createCollection('Books')
    ```

    <span data-ttu-id="fcadc-145">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="fcadc-145">The following result is displayed:</span></span>

    ```console
    { "ok" : 1 }
    ```

1. <span data-ttu-id="fcadc-146">Zdefiniować schemat `Books` kolekcji i Wstaw dwa dokumenty, używając następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="fcadc-146">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

    ```console
    db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
    ```

    <span data-ttu-id="fcadc-147">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="fcadc-147">The following result is displayed:</span></span>

    ```console
    {
      "acknowledged" : true,
      "insertedIds" : [
        ObjectId("5bfd996f7b8e48dc15ff215d"),
        ObjectId("5bfd996f7b8e48dc15ff215e")
      ]
    }
    ```

1. <span data-ttu-id="fcadc-148">Wyświetl dokumenty w bazie danych, używając następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="fcadc-148">View the documents in the database using the following command:</span></span>

    ```console
    db.Books.find({}).pretty()
    ```

    <span data-ttu-id="fcadc-149">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="fcadc-149">The following result is displayed:</span></span>

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

    <span data-ttu-id="fcadc-150">Schemat dodaje wygenerowany automatycznie `_id` właściwości typu `ObjectId` dla każdego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="fcadc-150">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="fcadc-151">Baza danych jest gotowy.</span><span class="sxs-lookup"><span data-stu-id="fcadc-151">The database is ready.</span></span> <span data-ttu-id="fcadc-152">Możesz rozpocząć tworzenie interfejsu API sieci web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fcadc-152">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="fcadc-153">Utwórz projekt interfejsu API sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fcadc-153">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fcadc-154">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fcadc-154">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="fcadc-155">Przejdź do **pliku** > **nowe** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="fcadc-155">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="fcadc-156">Wybierz **aplikacji sieci Web programu ASP.NET Core** typ projektu, a następnie wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="fcadc-156">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="fcadc-157">Nadaj projektowi nazwę *BooksApi*i wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="fcadc-157">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="fcadc-158">Wybierz **platformy .NET Core** platformy docelowej i **platformy ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="fcadc-158">Select the **.NET Core** target framework and **ASP.NET Core 2.2**.</span></span> <span data-ttu-id="fcadc-159">Wybierz **API** projektu szablonu, a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="fcadc-159">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="fcadc-160">Odwiedź stronę [galerii pakietów NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) do określenia najnowszej stabilnej wersji sterownika platformy .NET dla bazy danych MongoDB.</span><span class="sxs-lookup"><span data-stu-id="fcadc-160">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="fcadc-161">W **Konsola Menedżera pakietów** okna, przejdź do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="fcadc-161">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="fcadc-162">Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:</span><span class="sxs-lookup"><span data-stu-id="fcadc-162">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```powershell
    Install-Package MongoDB.Driver -Version {VERSION}
    ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fcadc-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fcadc-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="fcadc-164">Uruchom następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="fcadc-164">Run the following commands in a command shell:</span></span>

    ```console
    dotnet new webapi -o BooksApi
    code BooksApi
    ```

    <span data-ttu-id="fcadc-165">Nowy projekt interfejsu API sieci web platformy ASP.NET Core, na przeznaczonych dla platformy .NET Core jest wygenerowany i otworzyć w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fcadc-165">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="fcadc-166">Po technologię OmniSharp pasek stanu gaśniczego ikona zmieni kolor na zielony, okno dialogowe prosi **"BooksApi" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?** .</span><span class="sxs-lookup"><span data-stu-id="fcadc-166">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="fcadc-167">Wybierz **tak**.</span><span class="sxs-lookup"><span data-stu-id="fcadc-167">Select **Yes**.</span></span>
1. <span data-ttu-id="fcadc-168">Odwiedź stronę [galerii pakietów NuGet: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) do określenia najnowszej stabilnej wersji sterownika platformy .NET dla bazy danych MongoDB.</span><span class="sxs-lookup"><span data-stu-id="fcadc-168">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="fcadc-169">Otwórz **zintegrowany Terminal** i przejdź do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="fcadc-169">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="fcadc-170">Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:</span><span class="sxs-lookup"><span data-stu-id="fcadc-170">Run the following command to install the .NET driver for MongoDB:</span></span>

    ```console
    dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
    ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fcadc-171">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="fcadc-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="fcadc-172">Przejdź do **pliku** > **nowe rozwiązanie** > **platformy .NET Core** > **aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="fcadc-172">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="fcadc-173">Wybierz **internetowego interfejsu API platformy ASP.NET Core** C# projektu szablonu, a następnie wybierz **dalej**.</span><span class="sxs-lookup"><span data-stu-id="fcadc-173">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="fcadc-174">Wybierz **platformy .NET Core 2.2** z **platformę docelową** listy rozwijanej i wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="fcadc-174">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="fcadc-175">Wprowadź *BooksApi* dla **Nazwa projektu**i wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="fcadc-175">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="fcadc-176">W **rozwiązania** konsoli kliknij prawym przyciskiem myszy projekt **zależności** a następnie wybierz węzeł **Dodawanie pakietów**.</span><span class="sxs-lookup"><span data-stu-id="fcadc-176">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="fcadc-177">Wprowadź *MongoDB.Driver* w polu wyszukiwania, wybierz *MongoDB.Driver* pakietu, a następnie wybierz pozycję **Dodaj pakiet**.</span><span class="sxs-lookup"><span data-stu-id="fcadc-177">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="fcadc-178">Wybierz **Akceptuj** znajdujący się w **akceptacja licencji** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="fcadc-178">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="fcadc-179">Dodawanie modelu jednostki</span><span class="sxs-lookup"><span data-stu-id="fcadc-179">Add an entity model</span></span>

1. <span data-ttu-id="fcadc-180">Dodaj *modeli* katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="fcadc-180">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="fcadc-181">Dodaj `Book` klasy *modeli* katalogu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fcadc-181">Add a `Book` class to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/Book.cs)]

    <span data-ttu-id="fcadc-182">W klasie poprzedniego `Id` właściwości:</span><span class="sxs-lookup"><span data-stu-id="fcadc-182">In the preceding class, the `Id` property:</span></span>
    
    * <span data-ttu-id="fcadc-183">Jest wymagany do mapowania obiektu środowiska uruchomieniowego języka wspólnego (CLR) do kolekcji usługi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="fcadc-183">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
    * <span data-ttu-id="fcadc-184">Jest oznaczony za pomocą [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) można określić tę właściwość jako klucz podstawowy dokumentu.</span><span class="sxs-lookup"><span data-stu-id="fcadc-184">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
    * <span data-ttu-id="fcadc-185">Jest oznaczony za pomocą [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) umożliwia przekazanie parametru jako typu `string` zamiast [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) struktury.</span><span class="sxs-lookup"><span data-stu-id="fcadc-185">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="fcadc-186">MONGO obsługuje konwersję z `string` do `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="fcadc-186">Mongo handles the conversion from `string` to `ObjectId`.</span></span>
    
    <span data-ttu-id="fcadc-187">Inne właściwości w klasie jest oznaczony za pomocą [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="fcadc-187">Other properties in the class are annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="fcadc-188">Wartość ten atrybut reprezentuje nazwę właściwości w kolekcji usługi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="fcadc-188">The attribute's value represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="fcadc-189">Dodaj model konfiguracji</span><span class="sxs-lookup"><span data-stu-id="fcadc-189">Add a configuration model</span></span>

1. <span data-ttu-id="fcadc-190">Dodaj następujące wartości konfiguracji bazy danych do *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="fcadc-190">Add the following database configuration values to *appsettings.json*:</span></span>

    [!code-json[](first-mongo-app/sample/BooksApi/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="fcadc-191">Dodaj *BookstoreDatabaseSettings.cs* plik *modeli* katalogu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fcadc-191">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Models/BookstoreDatabaseSettings.cs)]

    <span data-ttu-id="fcadc-192">Poprzedni `BookstoreDatabaseSettings` klasa jest używana do przechowywania *appsettings.json* pliku `BookstoreDatabaseSettings` wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="fcadc-192">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="fcadc-193">Za pomocą pliku JSON i C# nazwy właściwości są nazywane tak samo, aby ułatwić proces mapowania.</span><span class="sxs-lookup"><span data-stu-id="fcadc-193">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="fcadc-194">Dodaj następujący kod do `Startup.ConfigureServices`, przed wywołaniem do `AddMvc`:</span><span class="sxs-lookup"><span data-stu-id="fcadc-194">Add the following code to `Startup.ConfigureServices`, before the call to `AddMvc`:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureDatabaseSettings)]

    <span data-ttu-id="fcadc-195">W poprzednim kodzie:</span><span class="sxs-lookup"><span data-stu-id="fcadc-195">In the preceding code:</span></span>

    * <span data-ttu-id="fcadc-196">Wystąpienia konfiguracji, do którego *appsettings.json* pliku `BookstoreDatabaseSettings` powiązań sekcji jest zarejestrowany w kontenerze wstrzykiwanie zależności (DI).</span><span class="sxs-lookup"><span data-stu-id="fcadc-196">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="fcadc-197">Na przykład `BookstoreDatabaseSettings` obiektu `ConnectionString` właściwość jest wypełniana przy użyciu `BookstoreDatabaseSettings:ConnectionString` właściwość *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="fcadc-197">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
    * <span data-ttu-id="fcadc-198">`IBookstoreDatabaseSettings` Interfejs jest zarejestrowany w DI za pomocą pojedynczego [okres istnienia usługi](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="fcadc-198">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="fcadc-199">Gdy dodane wystąpienie interfejsu jest rozpoznawana jako `BookstoreDatabaseSettings` obiektu.</span><span class="sxs-lookup"><span data-stu-id="fcadc-199">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="fcadc-200">Dodaj następujący kod na górze *Startup.cs* rozpoznać `BookstoreDatabaseSettings` i `IBookstoreDatabaseSettings` odwołania:</span><span class="sxs-lookup"><span data-stu-id="fcadc-200">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="fcadc-201">Dodawanie obsługi operacji CRUD</span><span class="sxs-lookup"><span data-stu-id="fcadc-201">Add a CRUD operations service</span></span>

1. <span data-ttu-id="fcadc-202">Dodaj *usług* katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="fcadc-202">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="fcadc-203">Dodaj `BookService` klasy *usług* katalogu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fcadc-203">Add a `BookService` class to the *Services* directory with the following code:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceClass)]

    <span data-ttu-id="fcadc-204">W poprzednim kodzie `IBookstoreDatabaseSettings` wystąpienia jest pobierana z DI przy użyciu iniekcji konstruktora.</span><span class="sxs-lookup"><span data-stu-id="fcadc-204">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="fcadc-205">Ta metoda zapewnia dostęp do *appsettings.json* wartości konfiguracji, które zostały dodane w [Dodaj model konfiguracji](#add-a-configuration-model) sekcji.</span><span class="sxs-lookup"><span data-stu-id="fcadc-205">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="fcadc-206">W `Startup.ConfigureServices`, zarejestruj `BookService` klasie z atrybutem DI:</span><span class="sxs-lookup"><span data-stu-id="fcadc-206">In `Startup.ConfigureServices`, register the `BookService` class with DI:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_ConfigureServices&highlight=9)]

    <span data-ttu-id="fcadc-207">W poprzednim kodzie `BookService` klasy jest zarejestrowane w usłudze DI w celu obsługi iniekcji konstruktora w korzystających z tych klas.</span><span class="sxs-lookup"><span data-stu-id="fcadc-207">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="fcadc-208">Okres istnienia usługi singleton jest najbardziej odpowiednia ponieważ `BookService` zdobywa bezpośredniej zależności `MongoClient`.</span><span class="sxs-lookup"><span data-stu-id="fcadc-208">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="fcadc-209">Na official będzie przydatna [wytycznych ponowne użycie klienta Mongo](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` powinien być zarejestrowany w DI o okresie istnienia usługi singleton.</span><span class="sxs-lookup"><span data-stu-id="fcadc-209">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="fcadc-210">Dodaj następujący kod na górze *Startup.cs* rozpoznać `BookService` odwołania:</span><span class="sxs-lookup"><span data-stu-id="fcadc-210">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="fcadc-211">`BookService` Klasa używa następujących `MongoDB.Driver` elementy członkowskie do wykonywania operacji CRUD w odniesieniu do bazy danych:</span><span class="sxs-lookup"><span data-stu-id="fcadc-211">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="fcadc-212">[Klucza MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; odczytuje wystąpienie serwera do wykonywania operacji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fcadc-212">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="fcadc-213">Konstruktor obiektu tej klasy znajduje się ciąg połączenia bazy danych MongoDB:</span><span class="sxs-lookup"><span data-stu-id="fcadc-213">The constructor of this class is provided the MongoDB connection string:</span></span>

    [!code-csharp[](first-mongo-app/sample/BooksApi/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="fcadc-214">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; reprezentuje bazą danych Mongo do wykonywania operacji.</span><span class="sxs-lookup"><span data-stu-id="fcadc-214">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="fcadc-215">W tym samouczku korzysta z ogólnego [GetCollection<TDocument>(kolekcję)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) metodę interfejsu, aby uzyskać dostęp do danych w określonej kolekcji.</span><span class="sxs-lookup"><span data-stu-id="fcadc-215">This tutorial uses the generic [GetCollection<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="fcadc-216">Wykonywanie operacji CRUD względem kolekcji należy wykonać po ta metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="fcadc-216">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="fcadc-217">W `GetCollection<TDocument>(collection)` wywołanie metody:</span><span class="sxs-lookup"><span data-stu-id="fcadc-217">In the `GetCollection<TDocument>(collection)` method call:</span></span>
  * <span data-ttu-id="fcadc-218">`collection` reprezentuje nazwę kolekcji.</span><span class="sxs-lookup"><span data-stu-id="fcadc-218">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="fcadc-219">`TDocument` reprezentuje typ obiektu CLR, które są przechowywane w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="fcadc-219">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="fcadc-220">`GetCollection<TDocument>(collection)` Zwraca [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) obiekt reprezentujący kolekcję.</span><span class="sxs-lookup"><span data-stu-id="fcadc-220">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="fcadc-221">W tym samouczku następujące metody są wywoływane w kolekcji:</span><span class="sxs-lookup"><span data-stu-id="fcadc-221">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="fcadc-222">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; usuwa zgodne z kryteriami wyszukiwania podana pojedynczego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="fcadc-222">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="fcadc-223">[Znajdź\<TDocument >](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; zwraca wszystkie dokumenty w kolekcji, zgodne z kryteriami wyszukiwania podana.</span><span class="sxs-lookup"><span data-stu-id="fcadc-223">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="fcadc-224">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Wstawia podany obiekt jako nowy dokument w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="fcadc-224">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="fcadc-225">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; zastępuje jednolitego dokumentu, które są zgodne z kryteriami wyszukiwania podanej za pomocą udostępnionego obiektu.</span><span class="sxs-lookup"><span data-stu-id="fcadc-225">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="fcadc-226">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="fcadc-226">Add a controller</span></span>

<span data-ttu-id="fcadc-227">Dodaj `BooksController` klasy *kontrolerów* katalogu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="fcadc-227">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/sample/BooksApi/Controllers/BooksController.cs)]

<span data-ttu-id="fcadc-228">Poprzedni kontroler internetowego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="fcadc-228">The preceding web API controller:</span></span>

* <span data-ttu-id="fcadc-229">Używa `BookService` klasy w celu wykonywania operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="fcadc-229">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="fcadc-230">Zawiera metody akcji w celu obsługi żądań GET, POST, PUT i DELETE protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="fcadc-230">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="fcadc-231">Wywołania <xref:System.Web.Http.ApiController.CreatedAtRoute*> w `Create` metody akcji do zwrócenia [201 protokołu HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="fcadc-231">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="fcadc-232">Kod stanu 201 to standardowa odpowiedź na metodę POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="fcadc-232">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="fcadc-233">`CreatedAtRoute` dodaje także `Location` nagłówka odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="fcadc-233">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="fcadc-234">`Location` Nagłówek Określa identyfikator URI nowo utworzonego książki.</span><span class="sxs-lookup"><span data-stu-id="fcadc-234">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="fcadc-235">Testowanie interfejsu API sieci web</span><span class="sxs-lookup"><span data-stu-id="fcadc-235">Test the web API</span></span>

1. <span data-ttu-id="fcadc-236">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="fcadc-236">Build and run the app.</span></span>

1. <span data-ttu-id="fcadc-237">Przejdź do `http://localhost:<port>/api/books` do kontrolera testowego użytkownika bez parametrów `Get` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="fcadc-237">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="fcadc-238">Wyświetlane są następujące odpowiedź w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="fcadc-238">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="fcadc-239">Przejdź do `http://localhost:<port>/api/books/5bfd996f7b8e48dc15ff215e` do kontrolera testowego użytkownika przeciążone `Get` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="fcadc-239">Navigate to `http://localhost:<port>/api/books/5bfd996f7b8e48dc15ff215e` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="fcadc-240">Wyświetlane są następujące odpowiedź w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="fcadc-240">The following JSON response is displayed:</span></span>

    ```json
    {
      "id":"5bfd996f7b8e48dc15ff215e",
      "bookName":"Clean Code",
      "price":43.15,
      "category":"Computers",
      "author":"Robert C. Martin"
    }
    ```

## <a name="next-steps"></a><span data-ttu-id="fcadc-241">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="fcadc-241">Next steps</span></span>

<span data-ttu-id="fcadc-242">Aby uzyskać więcej informacji dotyczących tworzenia interfejsów API sieci web platformy ASP.NET Core zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="fcadc-242">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="fcadc-243">YouTube wersję tego artykułu</span><span class="sxs-lookup"><span data-stu-id="fcadc-243">YouTube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
