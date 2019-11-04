---
title: Tworzenie internetowego interfejsu API za pomocą ASP.NET Core i MongoDB
author: prkhandelwal
description: W tym samouczku przedstawiono sposób tworzenia ASP.NET Core internetowego interfejsu API przy użyciu bazy danych NoSQL MongoDB.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 08/17/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: 42c0efcd914eaa54134827cdf3bd6bd599d512b2
ms.sourcegitcommit: 77c8be22d5e88dd710f42c739748869f198865dd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2019
ms.locfileid: "73426995"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="03774-103">Tworzenie internetowego interfejsu API za pomocą ASP.NET Core i MongoDB</span><span class="sxs-lookup"><span data-stu-id="03774-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="03774-104">Autorzy [Pratik Khandelwal](https://twitter.com/K2Prk) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="03774-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="03774-105">W tym samouczku przedstawiono Tworzenie interfejsu API sieci Web, który wykonuje operacje tworzenia, odczytu, aktualizacji i usuwania (CRUD) w bazie danych [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL.</span><span class="sxs-lookup"><span data-stu-id="03774-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="03774-106">Z tego samouczka dowiesz się, jak wykonywać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="03774-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="03774-107">Konfigurowanie MongoDB</span><span class="sxs-lookup"><span data-stu-id="03774-107">Configure MongoDB</span></span>
> * <span data-ttu-id="03774-108">Tworzenie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="03774-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="03774-109">Zdefiniuj kolekcję MongoDB i schemat</span><span class="sxs-lookup"><span data-stu-id="03774-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="03774-110">Wykonywanie operacji MongoDB CRUD z internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="03774-110">Perform MongoDB CRUD operations from a web API</span></span>
> * <span data-ttu-id="03774-111">Dostosowywanie serializacji JSON</span><span class="sxs-lookup"><span data-stu-id="03774-111">Customize JSON serialization</span></span>

<span data-ttu-id="03774-112">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="03774-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="03774-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="03774-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="03774-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="03774-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="03774-115">Zestaw .NET Core SDK 3,0 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="03774-115">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="03774-116">[Program Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z **ASP.NET i programowaniem aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="03774-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="03774-117">MongoDB</span><span class="sxs-lookup"><span data-stu-id="03774-117">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="03774-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="03774-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="03774-119">Zestaw .NET Core SDK 3,0 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="03774-119">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="03774-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="03774-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="03774-121">C#dla Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="03774-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="03774-122">MongoDB</span><span class="sxs-lookup"><span data-stu-id="03774-122">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="03774-123">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="03774-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="03774-124">Zestaw .NET Core SDK 3,0 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="03774-124">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="03774-125">Visual Studio dla komputerów Mac wersja 7,7 lub nowsza</span><span class="sxs-lookup"><span data-stu-id="03774-125">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="03774-126">MongoDB</span><span class="sxs-lookup"><span data-stu-id="03774-126">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="03774-127">Konfigurowanie MongoDB</span><span class="sxs-lookup"><span data-stu-id="03774-127">Configure MongoDB</span></span>

<span data-ttu-id="03774-128">W przypadku korzystania z systemu Windows MongoDB jest instalowany w lokalizacji *C:\\Program Files\\* domyślnie.</span><span class="sxs-lookup"><span data-stu-id="03774-128">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="03774-129">Dodaj *pliki C:\\programów\\MongoDB\\Server\\\<version_number >\\bin* do zmiennej środowiskowej `Path`.</span><span class="sxs-lookup"><span data-stu-id="03774-129">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="03774-130">Ta zmiana umożliwia MongoDB dostęp z dowolnego miejsca na komputerze deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="03774-130">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="03774-131">Użyj powłoki Mongo w poniższych krokach, aby utworzyć bazę danych, utworzyć kolekcje i przechowywać dokumenty.</span><span class="sxs-lookup"><span data-stu-id="03774-131">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="03774-132">Aby uzyskać więcej informacji na temat poleceń powłoki Mongo, zobacz [Praca z powłoką Mongo](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="03774-132">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="03774-133">Wybierz katalog na komputerze deweloperskim, który ma być używany do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="03774-133">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="03774-134">Na przykład *C:\\BooksData* w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="03774-134">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="03774-135">Utwórz katalog, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="03774-135">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="03774-136">Powłoka Mongo nie tworzy nowych katalogów.</span><span class="sxs-lookup"><span data-stu-id="03774-136">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="03774-137">Otwórz powłokę poleceń.</span><span class="sxs-lookup"><span data-stu-id="03774-137">Open a command shell.</span></span> <span data-ttu-id="03774-138">Uruchom następujące polecenie, aby nawiązać połączenie z usługą MongoDB na domyślnym porcie 27017.</span><span class="sxs-lookup"><span data-stu-id="03774-138">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="03774-139">Pamiętaj, aby zamienić `<data_directory_path>` na katalog wybrany w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="03774-139">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

   ```console
   mongod --dbpath <data_directory_path>
   ```

1. <span data-ttu-id="03774-140">Otwórz inne wystąpienie powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="03774-140">Open another command shell instance.</span></span> <span data-ttu-id="03774-141">Połącz się z domyślną bazą danych testów, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="03774-141">Connect to the default test database by running the following command:</span></span>

   ```console
   mongo
   ```

1. <span data-ttu-id="03774-142">Uruchom następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="03774-142">Run the following in a command shell:</span></span>

   ```console
   use BookstoreDb
   ```

   <span data-ttu-id="03774-143">Jeśli jeszcze nie istnieje, zostanie utworzona baza danych o nazwie *BookstoreDb* .</span><span class="sxs-lookup"><span data-stu-id="03774-143">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="03774-144">Jeśli baza danych istnieje, jego połączenie jest otwierane dla transakcji.</span><span class="sxs-lookup"><span data-stu-id="03774-144">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="03774-145">Utwórz kolekcję `Books` przy użyciu następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="03774-145">Create a `Books` collection using following command:</span></span>

   ```console
   db.createCollection('Books')
   ```

   <span data-ttu-id="03774-146">Zostanie wyświetlony następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="03774-146">The following result is displayed:</span></span>

   ```console
   { "ok" : 1 }
   ```

1. <span data-ttu-id="03774-147">Zdefiniuj schemat dla kolekcji `Books` i Wstaw dwa dokumenty przy użyciu następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="03774-147">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

   ```console
   db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
   ```

   <span data-ttu-id="03774-148">Zostanie wyświetlony następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="03774-148">The following result is displayed:</span></span>

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
   > <span data-ttu-id="03774-149">Identyfikator przedstawiony w tym artykule nie będzie pasował do identyfikatorów podczas uruchamiania tego przykładu.</span><span class="sxs-lookup"><span data-stu-id="03774-149">The ID's shown in this article will not match the IDs when you run this sample.</span></span>

1. <span data-ttu-id="03774-150">Wyświetl dokumenty w bazie danych przy użyciu następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="03774-150">View the documents in the database using the following command:</span></span>

   ```console
   db.Books.find({}).pretty()
   ```

   <span data-ttu-id="03774-151">Zostanie wyświetlony następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="03774-151">The following result is displayed:</span></span>

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

   <span data-ttu-id="03774-152">Schemat dodaje automatycznie wygenerowaną Właściwość `_id` typu `ObjectId` dla każdego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="03774-152">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="03774-153">Baza danych jest gotowa.</span><span class="sxs-lookup"><span data-stu-id="03774-153">The database is ready.</span></span> <span data-ttu-id="03774-154">Możesz rozpocząć tworzenie ASP.NET Core internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="03774-154">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="03774-155">Tworzenie projektu interfejsu API sieci Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="03774-155">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="03774-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="03774-156">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="03774-157">Przejdź do **pliku** > **nowym** > **projekcie**.</span><span class="sxs-lookup"><span data-stu-id="03774-157">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="03774-158">Wybierz typ projektu **aplikacja sieci Web ASP.NET Core** a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="03774-158">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="03774-159">Nazwij projekt *BooksApi*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="03774-159">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="03774-160">Wybierz platformę docelową **.NET Core** i **ASP.NET Core 3,0**.</span><span class="sxs-lookup"><span data-stu-id="03774-160">Select the **.NET Core** target framework and **ASP.NET Core 3.0**.</span></span> <span data-ttu-id="03774-161">Wybierz szablon projektu **interfejsu API** i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="03774-161">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="03774-162">Odwiedź [galerię NuGet: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) , aby określić najnowszą stabilną wersję sterownika .NET dla usługi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="03774-162">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="03774-163">W oknie **konsola Menedżera pakietów** przejdź do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="03774-163">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="03774-164">Uruchom następujące polecenie, aby zainstalować sterownik .NET dla MongoDB:</span><span class="sxs-lookup"><span data-stu-id="03774-164">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```powershell
   Install-Package MongoDB.Driver -Version {VERSION}
   ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="03774-165">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="03774-165">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="03774-166">Uruchom następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="03774-166">Run the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new webapi -o BooksApi
   code BooksApi
   ```

   <span data-ttu-id="03774-167">Nowy projekt interfejsu API sieci Web ASP.NET Core przeznaczony dla platformy .NET Core został wygenerowany i otwarty w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="03774-167">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="03774-168">Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w oknie dialogowym zostanie wyświetlony monit **o podanie wymaganych zasobów do skompilowania i debugowania z elementu "BooksApi". Dodać je?** .</span><span class="sxs-lookup"><span data-stu-id="03774-168">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="03774-169">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="03774-169">Select **Yes**.</span></span>
1. <span data-ttu-id="03774-170">Odwiedź [galerię NuGet: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) , aby określić najnowszą stabilną wersję sterownika .NET dla usługi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="03774-170">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="03774-171">Otwórz **zintegrowany terminal** i przejdź do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="03774-171">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="03774-172">Uruchom następujące polecenie, aby zainstalować sterownik .NET dla MongoDB:</span><span class="sxs-lookup"><span data-stu-id="03774-172">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```dotnetcli
   dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="03774-173">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="03774-173">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="03774-174">Przejdź do **pliku** > **nowe rozwiązanie** > **.NET Core** > **App**.</span><span class="sxs-lookup"><span data-stu-id="03774-174">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="03774-175">Wybierz szablon projektu C# **interfejsu API sieci Web ASP.NET Core** i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="03774-175">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="03774-176">Z listy rozwijanej **platforma docelowa** wybierz pozycję **.NET Core 3,0** , a następnie wybierz pozycję **Next (dalej**).</span><span class="sxs-lookup"><span data-stu-id="03774-176">Select **.NET Core 3.0** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="03774-177">Wprowadź *BooksApi* jako **nazwę projektu**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="03774-177">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="03774-178">W konsoli **rozwiązania** kliknij prawym przyciskiem myszy węzeł **zależności** projektu i wybierz polecenie **Dodaj pakiety**.</span><span class="sxs-lookup"><span data-stu-id="03774-178">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="03774-179">Wprowadź *MongoDB. Driver* w polu wyszukiwania, wybierz pakiet *MongoDB. Driver* , a następnie wybierz pozycję **Dodaj pakiet**.</span><span class="sxs-lookup"><span data-stu-id="03774-179">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="03774-180">Wybierz przycisk **Akceptuj** w oknie dialogowym **akceptacji licencji** .</span><span class="sxs-lookup"><span data-stu-id="03774-180">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="03774-181">Dodaj model jednostki</span><span class="sxs-lookup"><span data-stu-id="03774-181">Add an entity model</span></span>

1. <span data-ttu-id="03774-182">Dodaj katalog *models* do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="03774-182">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="03774-183">Dodaj klasę `Book` do katalogu *models* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="03774-183">Add a `Book` class to the *Models* directory with the following code:</span></span>

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

   <span data-ttu-id="03774-184">W poprzedniej klasie Właściwość `Id`:</span><span class="sxs-lookup"><span data-stu-id="03774-184">In the preceding class, the `Id` property:</span></span>

   * <span data-ttu-id="03774-185">Jest wymagany do mapowania obiektu środowiska uruchomieniowego języka wspólnego (CLR) do kolekcji MongoDB.</span><span class="sxs-lookup"><span data-stu-id="03774-185">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
   * <span data-ttu-id="03774-186">Zawiera adnotację z [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) , aby wyznaczyć tę właściwość jako klucz podstawowy dokumentu.</span><span class="sxs-lookup"><span data-stu-id="03774-186">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
   * <span data-ttu-id="03774-187">Zawiera adnotację z [[BsonRepresentation (BsonType. objectid)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) , aby zezwolić na przekazywanie parametru jako typ `string` zamiast struktury [objectid](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) .</span><span class="sxs-lookup"><span data-stu-id="03774-187">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="03774-188">Mongo obsługuje konwersję z `string` na `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="03774-188">Mongo handles the conversion from `string` to `ObjectId`.</span></span>

   <span data-ttu-id="03774-189">Właściwość `BookName` ma adnotację z atrybutem [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) .</span><span class="sxs-lookup"><span data-stu-id="03774-189">The `BookName` property is annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="03774-190">Wartość atrybutu `Name` reprezentuje nazwę właściwości w kolekcji MongoDB.</span><span class="sxs-lookup"><span data-stu-id="03774-190">The attribute's value of `Name` represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="03774-191">Dodaj model konfiguracji</span><span class="sxs-lookup"><span data-stu-id="03774-191">Add a configuration model</span></span>

1. <span data-ttu-id="03774-192">Dodaj następujące wartości konfiguracji bazy danych do pliku *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="03774-192">Add the following database configuration values to *appsettings.json*:</span></span>

   [!code-json[](first-mongo-app/samples/3.x/SampleApp/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="03774-193">Dodaj plik *BookstoreDatabaseSettings.cs* do katalogu *models* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="03774-193">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/BookstoreDatabaseSettings.cs)]

   <span data-ttu-id="03774-194">Poprzednia Klasa `BookstoreDatabaseSettings` jest używana do przechowywania wartości właściwości `BookstoreDatabaseSettings` pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="03774-194">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="03774-195">Nazwy JSON i C# właściwości są nazywane identycznie, aby uprościć proces mapowania.</span><span class="sxs-lookup"><span data-stu-id="03774-195">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="03774-196">Dodaj następujący wyróżniony kod do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="03774-196">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/3.x/SampleApp/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

   <span data-ttu-id="03774-197">W powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="03774-197">In the preceding code:</span></span>

   * <span data-ttu-id="03774-198">Wystąpienie konfiguracji, do którego są powiązane sekcja `BookstoreDatabaseSettings` pliku *appSettings. JSON* , jest zarejestrowane w kontenerze iniekcji zależności (di).</span><span class="sxs-lookup"><span data-stu-id="03774-198">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="03774-199">Na przykład właściwość `ConnectionString` obiektu `BookstoreDatabaseSettings` jest wypełniana właściwością `BookstoreDatabaseSettings:ConnectionString` w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="03774-199">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
   * <span data-ttu-id="03774-200">Interfejs `IBookstoreDatabaseSettings` jest rejestrowany przy użyciu programu DI z pojedynczym [okresem istnienia usługi](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="03774-200">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="03774-201">Po dowstrzykiwaniu wystąpienie interfejsu jest rozpoznawane jako obiekt `BookstoreDatabaseSettings`.</span><span class="sxs-lookup"><span data-stu-id="03774-201">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="03774-202">Dodaj następujący kod na początku *Startup.cs* , aby rozwiązać `BookstoreDatabaseSettings` i `IBookstoreDatabaseSettings` odwołania:</span><span class="sxs-lookup"><span data-stu-id="03774-202">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="03774-203">Dodawanie usługi operacji CRUD</span><span class="sxs-lookup"><span data-stu-id="03774-203">Add a CRUD operations service</span></span>

1. <span data-ttu-id="03774-204">Dodaj katalog *usług* do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="03774-204">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="03774-205">Dodaj klasę `BookService` do katalogu *usług* , korzystając z następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="03774-205">Add a `BookService` class to the *Services* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceClass)]

   <span data-ttu-id="03774-206">W powyższym kodzie wystąpienie `IBookstoreDatabaseSettings` jest pobierane z funkcji DI przez iniekcję konstruktora.</span><span class="sxs-lookup"><span data-stu-id="03774-206">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="03774-207">Ta technika zapewnia dostęp do wartości konfiguracyjnych *appSettings. JSON* , które zostały dodane w sekcji [Dodawanie modelu konfiguracji](#add-a-configuration-model) .</span><span class="sxs-lookup"><span data-stu-id="03774-207">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="03774-208">Dodaj następujący wyróżniony kod do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="03774-208">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/3.x/SampleApp/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

   <span data-ttu-id="03774-209">W poprzednim kodzie Klasa `BookService` jest zarejestrowana przy użyciu funkcji DI, aby obsługiwać iniekcję konstruktora w klasach zużywających.</span><span class="sxs-lookup"><span data-stu-id="03774-209">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="03774-210">Okres istnienia usługi pojedynczej jest najbardziej odpowiedni, ponieważ `BookService` wykonuje bezpośrednią zależność od `MongoClient`.</span><span class="sxs-lookup"><span data-stu-id="03774-210">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="03774-211">Zgodnie z oficjalnymi [wytycznymi klienta Mongo](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)należy zarejestrować `MongoClient` w programie di z pojedynczym okresem istnienia usługi.</span><span class="sxs-lookup"><span data-stu-id="03774-211">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="03774-212">Dodaj następujący kod na górze elementu *Startup.cs* , aby rozwiązać `BookService` odwołanie:</span><span class="sxs-lookup"><span data-stu-id="03774-212">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="03774-213">Klasa `BookService` używa następujących członków `MongoDB.Driver` do wykonywania operacji CRUD w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="03774-213">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="03774-214">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; odczytuje wystąpienie serwera na potrzeby wykonywania operacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="03774-214">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="03774-215">W konstruktorze tej klasy podano parametry połączenia MongoDB:</span><span class="sxs-lookup"><span data-stu-id="03774-215">The constructor of this class is provided the MongoDB connection string:</span></span>

  [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="03774-216">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; reprezentuje bazę danych Mongo do wykonywania operacji.</span><span class="sxs-lookup"><span data-stu-id="03774-216">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="03774-217">W tym samouczku do uzyskiwania dostępu do danych w określonej kolekcji jest stosowana Metoda ogólna [getcollection\<TDocument > (Collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) .</span><span class="sxs-lookup"><span data-stu-id="03774-217">This tutorial uses the generic [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="03774-218">Wykonaj operacje CRUD w odniesieniu do kolekcji po wywołaniu tej metody.</span><span class="sxs-lookup"><span data-stu-id="03774-218">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="03774-219">W wywołaniu metody `GetCollection<TDocument>(collection)`:</span><span class="sxs-lookup"><span data-stu-id="03774-219">In the `GetCollection<TDocument>(collection)` method call:</span></span>

  * <span data-ttu-id="03774-220">`collection` reprezentuje nazwę kolekcji.</span><span class="sxs-lookup"><span data-stu-id="03774-220">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="03774-221">`TDocument` reprezentuje typ obiektu CLR przechowywany w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="03774-221">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="03774-222">`GetCollection<TDocument>(collection)` zwraca obiekt [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) reprezentujący kolekcję.</span><span class="sxs-lookup"><span data-stu-id="03774-222">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="03774-223">W tym samouczku następujące metody są wywoływane w kolekcji:</span><span class="sxs-lookup"><span data-stu-id="03774-223">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="03774-224">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; usuwa pojedynczy dokument pasujący do podanych kryteriów wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="03774-224">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="03774-225">[Znajdź\<TDocument >](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; zwraca wszystkie dokumenty w kolekcji pasujące do podanych kryteriów wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="03774-225">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="03774-226">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Wstawia podany obiekt jako nowy dokument w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="03774-226">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="03774-227">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; zastępuje pojedynczy dokument pasujący do podanych kryteriów wyszukiwania z podanym obiektem.</span><span class="sxs-lookup"><span data-stu-id="03774-227">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="03774-228">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="03774-228">Add a controller</span></span>

<span data-ttu-id="03774-229">Dodaj klasę `BooksController` do katalogu *controllers* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="03774-229">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Controllers/BooksController.cs)]

<span data-ttu-id="03774-230">Poprzedni kontroler interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="03774-230">The preceding web API controller:</span></span>

* <span data-ttu-id="03774-231">Używa klasy `BookService` do wykonywania operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="03774-231">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="03774-232">Zawiera metody akcji do obsługi żądań HTTP GET, POST, PUT i DELETE.</span><span class="sxs-lookup"><span data-stu-id="03774-232">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="03774-233">Wywołuje <xref:System.Web.Http.ApiController.CreatedAtRoute*> w metodzie `Create`, aby zwrócić odpowiedź [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) .</span><span class="sxs-lookup"><span data-stu-id="03774-233">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="03774-234">Kod stanu 201 jest standardową odpowiedzią dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="03774-234">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="03774-235">`CreatedAtRoute` dodaje również nagłówek `Location` do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="03774-235">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="03774-236">Nagłówek `Location` określa identyfikator URI nowo utworzonej książki.</span><span class="sxs-lookup"><span data-stu-id="03774-236">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="03774-237">Testowanie interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="03774-237">Test the web API</span></span>

1. <span data-ttu-id="03774-238">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="03774-238">Build and run the app.</span></span>

1. <span data-ttu-id="03774-239">Przejdź do `http://localhost:<port>/api/books`, aby przetestować bezparametryczne `Get` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="03774-239">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="03774-240">Zostanie wyświetlona następująca odpowiedź JSON:</span><span class="sxs-lookup"><span data-stu-id="03774-240">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="03774-241">Przejdź do `http://localhost:<port>/api/books/{id here}`, aby przetestować metodę `Get` akcji przeciążonej kontrolera.</span><span class="sxs-lookup"><span data-stu-id="03774-241">Navigate to `http://localhost:<port>/api/books/{id here}` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="03774-242">Zostanie wyświetlona następująca odpowiedź JSON:</span><span class="sxs-lookup"><span data-stu-id="03774-242">The following JSON response is displayed:</span></span>

   ```json
   {
     "id":"{ID}",
     "bookName":"Clean Code",
     "price":43.15,
     "category":"Computers",
     "author":"Robert C. Martin"
   }
   ```

## <a name="configure-json-serialization-options"></a><span data-ttu-id="03774-243">Konfigurowanie opcji serializacji JSON</span><span class="sxs-lookup"><span data-stu-id="03774-243">Configure JSON serialization options</span></span>

<span data-ttu-id="03774-244">Istnieją dwa szczegóły dotyczące odpowiedzi JSON zwróconych w sekcji [testowanie interfejsu API sieci Web](#test-the-web-api) :</span><span class="sxs-lookup"><span data-stu-id="03774-244">There are two details to change about the JSON responses returned in the [Test the web API](#test-the-web-api) section:</span></span>

* <span data-ttu-id="03774-245">Nazwy właściwości "default notacji CamelCase wielkość liter należy zmienić tak, aby pasowały do wielkości liter w języku Pascal nazw właściwości obiektu CLR.</span><span class="sxs-lookup"><span data-stu-id="03774-245">The property names' default camel casing should be changed to match the Pascal casing of the CLR object's property names.</span></span>
* <span data-ttu-id="03774-246">Właściwość `bookName` powinna zostać zwrócona jako `Name`.</span><span class="sxs-lookup"><span data-stu-id="03774-246">The `bookName` property should be returned as `Name`.</span></span>

<span data-ttu-id="03774-247">Aby spełnić powyższe wymagania, należy wprowadzić następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="03774-247">To satisfy the preceding requirements, make the following changes:</span></span>

1. <span data-ttu-id="03774-248">JSON.NET został usunięty z ASP.NET udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="03774-248">JSON.NET has been removed from ASP.NET shared framework.</span></span> <span data-ttu-id="03774-249">Dodaj odwołanie do pakietu do [Microsoft. AspNetCore. MVC. NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson).</span><span class="sxs-lookup"><span data-stu-id="03774-249">Add a package reference to [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson).</span></span>

1. <span data-ttu-id="03774-250">W `Startup.ConfigureServices`łańcuchować następujący wyróżniony kod do wywołania metody `AddMvc`:</span><span class="sxs-lookup"><span data-stu-id="03774-250">In `Startup.ConfigureServices`, chain the following highlighted code on to the `AddMvc` method call:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

   <span data-ttu-id="03774-251">W przypadku poprzedniej zmiany nazwy właściwości w serializowanej odpowiedzi JSON interfejsu API sieci Web pasują do odpowiednich nazw właściwości w typie obiektu CLR.</span><span class="sxs-lookup"><span data-stu-id="03774-251">With the preceding change, property names in the web API's serialized JSON response match their corresponding property names in the CLR object type.</span></span> <span data-ttu-id="03774-252">Na przykład Serializacja właściwości `Author` klasy `Book` jako `Author`.</span><span class="sxs-lookup"><span data-stu-id="03774-252">For example, the `Book` class's `Author` property serializes as `Author`.</span></span>

1. <span data-ttu-id="03774-253">W *modelach/książka. cs*Dodaj adnotację do właściwości `BookName` z następującym atrybutem [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) :</span><span class="sxs-lookup"><span data-stu-id="03774-253">In *Models/Book.cs*, annotate the `BookName` property with the following [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) attribute:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

   <span data-ttu-id="03774-254">Wartość `[JsonProperty]` atrybutu `Name` reprezentuje nazwę właściwości w serializowanej odpowiedzi JSON internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="03774-254">The `[JsonProperty]` attribute's value of `Name` represents the property name in the web API's serialized JSON response.</span></span>

1. <span data-ttu-id="03774-255">Dodaj następujący kod na górze *modeli/książek. cs* , aby rozwiązać `[JsonProperty]` odwołanie do atrybutu:</span><span class="sxs-lookup"><span data-stu-id="03774-255">Add the following code to the top of *Models/Book.cs* to resolve the `[JsonProperty]` attribute reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. <span data-ttu-id="03774-256">Powtórz kroki zdefiniowane w sekcji [testowanie interfejsu API sieci Web](#test-the-web-api) .</span><span class="sxs-lookup"><span data-stu-id="03774-256">Repeat the steps defined in the [Test the web API](#test-the-web-api) section.</span></span> <span data-ttu-id="03774-257">Zwróć uwagę na różnice w nazwach właściwości JSON.</span><span class="sxs-lookup"><span data-stu-id="03774-257">Notice the difference in JSON property names.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="03774-258">W tym samouczku przedstawiono Tworzenie interfejsu API sieci Web, który wykonuje operacje tworzenia, odczytu, aktualizacji i usuwania (CRUD) w bazie danych [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL.</span><span class="sxs-lookup"><span data-stu-id="03774-258">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="03774-259">Z tego samouczka dowiesz się, jak wykonywać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="03774-259">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="03774-260">Konfigurowanie MongoDB</span><span class="sxs-lookup"><span data-stu-id="03774-260">Configure MongoDB</span></span>
> * <span data-ttu-id="03774-261">Tworzenie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="03774-261">Create a MongoDB database</span></span>
> * <span data-ttu-id="03774-262">Zdefiniuj kolekcję MongoDB i schemat</span><span class="sxs-lookup"><span data-stu-id="03774-262">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="03774-263">Wykonywanie operacji MongoDB CRUD z internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="03774-263">Perform MongoDB CRUD operations from a web API</span></span>
> * <span data-ttu-id="03774-264">Dostosowywanie serializacji JSON</span><span class="sxs-lookup"><span data-stu-id="03774-264">Customize JSON serialization</span></span>

<span data-ttu-id="03774-265">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="03774-265">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="03774-266">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="03774-266">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="03774-267">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="03774-267">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="03774-268">Zestaw .NET Core SDK 2,2</span><span class="sxs-lookup"><span data-stu-id="03774-268">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="03774-269">[Program Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z **ASP.NET i programowaniem aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="03774-269">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="03774-270">MongoDB</span><span class="sxs-lookup"><span data-stu-id="03774-270">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="03774-271">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="03774-271">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="03774-272">Zestaw .NET Core SDK 2,2</span><span class="sxs-lookup"><span data-stu-id="03774-272">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="03774-273">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="03774-273">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="03774-274">C#dla Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="03774-274">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="03774-275">MongoDB</span><span class="sxs-lookup"><span data-stu-id="03774-275">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="03774-276">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="03774-276">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="03774-277">Zestaw .NET Core SDK 2,2</span><span class="sxs-lookup"><span data-stu-id="03774-277">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="03774-278">Visual Studio dla komputerów Mac wersja 7,7 lub nowsza</span><span class="sxs-lookup"><span data-stu-id="03774-278">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="03774-279">MongoDB</span><span class="sxs-lookup"><span data-stu-id="03774-279">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="03774-280">Konfigurowanie MongoDB</span><span class="sxs-lookup"><span data-stu-id="03774-280">Configure MongoDB</span></span>

<span data-ttu-id="03774-281">W przypadku korzystania z systemu Windows MongoDB jest instalowany w lokalizacji *C:\\Program Files\\* domyślnie.</span><span class="sxs-lookup"><span data-stu-id="03774-281">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="03774-282">Dodaj *pliki C:\\programów\\MongoDB\\Server\\\<version_number >\\bin* do zmiennej środowiskowej `Path`.</span><span class="sxs-lookup"><span data-stu-id="03774-282">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="03774-283">Ta zmiana umożliwia MongoDB dostęp z dowolnego miejsca na komputerze deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="03774-283">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="03774-284">Użyj powłoki Mongo w poniższych krokach, aby utworzyć bazę danych, utworzyć kolekcje i przechowywać dokumenty.</span><span class="sxs-lookup"><span data-stu-id="03774-284">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="03774-285">Aby uzyskać więcej informacji na temat poleceń powłoki Mongo, zobacz [Praca z powłoką Mongo](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="03774-285">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="03774-286">Wybierz katalog na komputerze deweloperskim, który ma być używany do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="03774-286">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="03774-287">Na przykład *C:\\BooksData* w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="03774-287">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="03774-288">Utwórz katalog, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="03774-288">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="03774-289">Powłoka Mongo nie tworzy nowych katalogów.</span><span class="sxs-lookup"><span data-stu-id="03774-289">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="03774-290">Otwórz powłokę poleceń.</span><span class="sxs-lookup"><span data-stu-id="03774-290">Open a command shell.</span></span> <span data-ttu-id="03774-291">Uruchom następujące polecenie, aby nawiązać połączenie z usługą MongoDB na domyślnym porcie 27017.</span><span class="sxs-lookup"><span data-stu-id="03774-291">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="03774-292">Pamiętaj, aby zamienić `<data_directory_path>` na katalog wybrany w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="03774-292">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

   ```console
   mongod --dbpath <data_directory_path>
   ```

1. <span data-ttu-id="03774-293">Otwórz inne wystąpienie powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="03774-293">Open another command shell instance.</span></span> <span data-ttu-id="03774-294">Połącz się z domyślną bazą danych testów, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="03774-294">Connect to the default test database by running the following command:</span></span>

   ```console
   mongo
   ```

1. <span data-ttu-id="03774-295">Uruchom następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="03774-295">Run the following in a command shell:</span></span>

   ```console
   use BookstoreDb
   ```

   <span data-ttu-id="03774-296">Jeśli jeszcze nie istnieje, zostanie utworzona baza danych o nazwie *BookstoreDb* .</span><span class="sxs-lookup"><span data-stu-id="03774-296">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="03774-297">Jeśli baza danych istnieje, jego połączenie jest otwierane dla transakcji.</span><span class="sxs-lookup"><span data-stu-id="03774-297">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="03774-298">Utwórz kolekcję `Books` przy użyciu następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="03774-298">Create a `Books` collection using following command:</span></span>

   ```console
   db.createCollection('Books')
   ```

   <span data-ttu-id="03774-299">Zostanie wyświetlony następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="03774-299">The following result is displayed:</span></span>

   ```console
   { "ok" : 1 }
   ```

1. <span data-ttu-id="03774-300">Zdefiniuj schemat dla kolekcji `Books` i Wstaw dwa dokumenty przy użyciu następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="03774-300">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

   ```console
   db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
   ```

   <span data-ttu-id="03774-301">Zostanie wyświetlony następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="03774-301">The following result is displayed:</span></span>

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
   > <span data-ttu-id="03774-302">Identyfikator przedstawiony w tym artykule nie będzie pasował do identyfikatorów podczas uruchamiania tego przykładu.</span><span class="sxs-lookup"><span data-stu-id="03774-302">The ID's shown in this article will not match the IDs when you run this sample.</span></span>

1. <span data-ttu-id="03774-303">Wyświetl dokumenty w bazie danych przy użyciu następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="03774-303">View the documents in the database using the following command:</span></span>

   ```console
   db.Books.find({}).pretty()
   ```

   <span data-ttu-id="03774-304">Zostanie wyświetlony następujący wynik:</span><span class="sxs-lookup"><span data-stu-id="03774-304">The following result is displayed:</span></span>

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

   <span data-ttu-id="03774-305">Schemat dodaje automatycznie wygenerowaną Właściwość `_id` typu `ObjectId` dla każdego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="03774-305">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="03774-306">Baza danych jest gotowa.</span><span class="sxs-lookup"><span data-stu-id="03774-306">The database is ready.</span></span> <span data-ttu-id="03774-307">Możesz rozpocząć tworzenie ASP.NET Core internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="03774-307">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="03774-308">Tworzenie projektu interfejsu API sieci Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="03774-308">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="03774-309">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="03774-309">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="03774-310">Przejdź do **pliku** > **nowym** > **projekcie**.</span><span class="sxs-lookup"><span data-stu-id="03774-310">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="03774-311">Wybierz typ projektu **aplikacja sieci Web ASP.NET Core** a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="03774-311">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="03774-312">Nazwij projekt *BooksApi*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="03774-312">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="03774-313">Wybierz platformę docelową **.NET Core** i **ASP.NET Core 2,2**.</span><span class="sxs-lookup"><span data-stu-id="03774-313">Select the **.NET Core** target framework and **ASP.NET Core 2.2**.</span></span> <span data-ttu-id="03774-314">Wybierz szablon projektu **interfejsu API** i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="03774-314">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="03774-315">Odwiedź [galerię NuGet: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) , aby określić najnowszą stabilną wersję sterownika .NET dla usługi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="03774-315">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="03774-316">W oknie **konsola Menedżera pakietów** przejdź do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="03774-316">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="03774-317">Uruchom następujące polecenie, aby zainstalować sterownik .NET dla MongoDB:</span><span class="sxs-lookup"><span data-stu-id="03774-317">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```powershell
   Install-Package MongoDB.Driver -Version {VERSION}
   ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="03774-318">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="03774-318">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="03774-319">Uruchom następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="03774-319">Run the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new webapi -o BooksApi
   code BooksApi
   ```

   <span data-ttu-id="03774-320">Nowy projekt interfejsu API sieci Web ASP.NET Core przeznaczony dla platformy .NET Core został wygenerowany i otwarty w Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="03774-320">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="03774-321">Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w oknie dialogowym zostanie wyświetlony monit **o podanie wymaganych zasobów do skompilowania i debugowania z elementu "BooksApi". Dodać je?** .</span><span class="sxs-lookup"><span data-stu-id="03774-321">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="03774-322">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="03774-322">Select **Yes**.</span></span>
1. <span data-ttu-id="03774-323">Odwiedź [galerię NuGet: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) , aby określić najnowszą stabilną wersję sterownika .NET dla usługi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="03774-323">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="03774-324">Otwórz **zintegrowany terminal** i przejdź do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="03774-324">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="03774-325">Uruchom następujące polecenie, aby zainstalować sterownik .NET dla MongoDB:</span><span class="sxs-lookup"><span data-stu-id="03774-325">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```dotnetcli
   dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="03774-326">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="03774-326">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="03774-327">Przejdź do **pliku** > **nowe rozwiązanie** > **.NET Core** > **App**.</span><span class="sxs-lookup"><span data-stu-id="03774-327">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="03774-328">Wybierz szablon projektu C# **interfejsu API sieci Web ASP.NET Core** i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="03774-328">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="03774-329">Z listy rozwijanej **platforma docelowa** wybierz pozycję **.NET Core 2,2** , a następnie wybierz pozycję **Next (dalej**).</span><span class="sxs-lookup"><span data-stu-id="03774-329">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="03774-330">Wprowadź *BooksApi* jako **nazwę projektu**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="03774-330">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="03774-331">W konsoli **rozwiązania** kliknij prawym przyciskiem myszy węzeł **zależności** projektu i wybierz polecenie **Dodaj pakiety**.</span><span class="sxs-lookup"><span data-stu-id="03774-331">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="03774-332">Wprowadź *MongoDB. Driver* w polu wyszukiwania, wybierz pakiet *MongoDB. Driver* , a następnie wybierz pozycję **Dodaj pakiet**.</span><span class="sxs-lookup"><span data-stu-id="03774-332">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="03774-333">Wybierz przycisk **Akceptuj** w oknie dialogowym **akceptacji licencji** .</span><span class="sxs-lookup"><span data-stu-id="03774-333">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="03774-334">Dodaj model jednostki</span><span class="sxs-lookup"><span data-stu-id="03774-334">Add an entity model</span></span>

1. <span data-ttu-id="03774-335">Dodaj katalog *models* do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="03774-335">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="03774-336">Dodaj klasę `Book` do katalogu *models* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="03774-336">Add a `Book` class to the *Models* directory with the following code:</span></span>

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

   <span data-ttu-id="03774-337">W poprzedniej klasie Właściwość `Id`:</span><span class="sxs-lookup"><span data-stu-id="03774-337">In the preceding class, the `Id` property:</span></span>

   * <span data-ttu-id="03774-338">Jest wymagany do mapowania obiektu środowiska uruchomieniowego języka wspólnego (CLR) do kolekcji MongoDB.</span><span class="sxs-lookup"><span data-stu-id="03774-338">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
   * <span data-ttu-id="03774-339">Zawiera adnotację z [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) , aby wyznaczyć tę właściwość jako klucz podstawowy dokumentu.</span><span class="sxs-lookup"><span data-stu-id="03774-339">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
   * <span data-ttu-id="03774-340">Zawiera adnotację z [[BsonRepresentation (BsonType. objectid)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) , aby zezwolić na przekazywanie parametru jako typ `string` zamiast struktury [objectid](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) .</span><span class="sxs-lookup"><span data-stu-id="03774-340">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="03774-341">Mongo obsługuje konwersję z `string` na `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="03774-341">Mongo handles the conversion from `string` to `ObjectId`.</span></span>

   <span data-ttu-id="03774-342">Właściwość `BookName` ma adnotację z atrybutem [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) .</span><span class="sxs-lookup"><span data-stu-id="03774-342">The `BookName` property is annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="03774-343">Wartość atrybutu `Name` reprezentuje nazwę właściwości w kolekcji MongoDB.</span><span class="sxs-lookup"><span data-stu-id="03774-343">The attribute's value of `Name` represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="03774-344">Dodaj model konfiguracji</span><span class="sxs-lookup"><span data-stu-id="03774-344">Add a configuration model</span></span>

1. <span data-ttu-id="03774-345">Dodaj następujące wartości konfiguracji bazy danych do pliku *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="03774-345">Add the following database configuration values to *appsettings.json*:</span></span>

   [!code-json[](first-mongo-app/samples/2.x/SampleApp/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="03774-346">Dodaj plik *BookstoreDatabaseSettings.cs* do katalogu *models* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="03774-346">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/BookstoreDatabaseSettings.cs)]

   <span data-ttu-id="03774-347">Poprzednia Klasa `BookstoreDatabaseSettings` jest używana do przechowywania wartości właściwości `BookstoreDatabaseSettings` pliku *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="03774-347">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="03774-348">Nazwy JSON i C# właściwości są nazywane identycznie, aby uprościć proces mapowania.</span><span class="sxs-lookup"><span data-stu-id="03774-348">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="03774-349">Dodaj następujący wyróżniony kod do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="03774-349">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/2.x/SampleApp/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

   <span data-ttu-id="03774-350">W powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="03774-350">In the preceding code:</span></span>

   * <span data-ttu-id="03774-351">Wystąpienie konfiguracji, do którego są powiązane sekcja `BookstoreDatabaseSettings` pliku *appSettings. JSON* , jest zarejestrowane w kontenerze iniekcji zależności (di).</span><span class="sxs-lookup"><span data-stu-id="03774-351">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="03774-352">Na przykład właściwość `ConnectionString` obiektu `BookstoreDatabaseSettings` jest wypełniana właściwością `BookstoreDatabaseSettings:ConnectionString` w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="03774-352">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
   * <span data-ttu-id="03774-353">Interfejs `IBookstoreDatabaseSettings` jest rejestrowany przy użyciu programu DI z pojedynczym [okresem istnienia usługi](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="03774-353">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="03774-354">Po dowstrzykiwaniu wystąpienie interfejsu jest rozpoznawane jako obiekt `BookstoreDatabaseSettings`.</span><span class="sxs-lookup"><span data-stu-id="03774-354">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="03774-355">Dodaj następujący kod na początku *Startup.cs* , aby rozwiązać `BookstoreDatabaseSettings` i `IBookstoreDatabaseSettings` odwołania:</span><span class="sxs-lookup"><span data-stu-id="03774-355">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="03774-356">Dodawanie usługi operacji CRUD</span><span class="sxs-lookup"><span data-stu-id="03774-356">Add a CRUD operations service</span></span>

1. <span data-ttu-id="03774-357">Dodaj katalog *usług* do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="03774-357">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="03774-358">Dodaj klasę `BookService` do katalogu *usług* , korzystając z następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="03774-358">Add a `BookService` class to the *Services* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceClass)]

   <span data-ttu-id="03774-359">W powyższym kodzie wystąpienie `IBookstoreDatabaseSettings` jest pobierane z funkcji DI przez iniekcję konstruktora.</span><span class="sxs-lookup"><span data-stu-id="03774-359">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="03774-360">Ta technika zapewnia dostęp do wartości konfiguracyjnych *appSettings. JSON* , które zostały dodane w sekcji [Dodawanie modelu konfiguracji](#add-a-configuration-model) .</span><span class="sxs-lookup"><span data-stu-id="03774-360">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="03774-361">Dodaj następujący wyróżniony kod do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="03774-361">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/2.x/SampleApp/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

   <span data-ttu-id="03774-362">W poprzednim kodzie Klasa `BookService` jest zarejestrowana przy użyciu funkcji DI, aby obsługiwać iniekcję konstruktora w klasach zużywających.</span><span class="sxs-lookup"><span data-stu-id="03774-362">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="03774-363">Okres istnienia usługi pojedynczej jest najbardziej odpowiedni, ponieważ `BookService` wykonuje bezpośrednią zależność od `MongoClient`.</span><span class="sxs-lookup"><span data-stu-id="03774-363">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="03774-364">Zgodnie z oficjalnymi [wytycznymi klienta Mongo](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)należy zarejestrować `MongoClient` w programie di z pojedynczym okresem istnienia usługi.</span><span class="sxs-lookup"><span data-stu-id="03774-364">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="03774-365">Dodaj następujący kod na górze elementu *Startup.cs* , aby rozwiązać `BookService` odwołanie:</span><span class="sxs-lookup"><span data-stu-id="03774-365">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="03774-366">Klasa `BookService` używa następujących członków `MongoDB.Driver` do wykonywania operacji CRUD w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="03774-366">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="03774-367">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; odczytuje wystąpienie serwera na potrzeby wykonywania operacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="03774-367">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="03774-368">W konstruktorze tej klasy podano parametry połączenia MongoDB:</span><span class="sxs-lookup"><span data-stu-id="03774-368">The constructor of this class is provided the MongoDB connection string:</span></span>

  [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="03774-369">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; reprezentuje bazę danych Mongo do wykonywania operacji.</span><span class="sxs-lookup"><span data-stu-id="03774-369">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="03774-370">W tym samouczku do uzyskiwania dostępu do danych w określonej kolekcji jest stosowana Metoda ogólna [getcollection\<TDocument > (Collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) .</span><span class="sxs-lookup"><span data-stu-id="03774-370">This tutorial uses the generic [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="03774-371">Wykonaj operacje CRUD w odniesieniu do kolekcji po wywołaniu tej metody.</span><span class="sxs-lookup"><span data-stu-id="03774-371">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="03774-372">W wywołaniu metody `GetCollection<TDocument>(collection)`:</span><span class="sxs-lookup"><span data-stu-id="03774-372">In the `GetCollection<TDocument>(collection)` method call:</span></span>

  * <span data-ttu-id="03774-373">`collection` reprezentuje nazwę kolekcji.</span><span class="sxs-lookup"><span data-stu-id="03774-373">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="03774-374">`TDocument` reprezentuje typ obiektu CLR przechowywany w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="03774-374">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="03774-375">`GetCollection<TDocument>(collection)` zwraca obiekt [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) reprezentujący kolekcję.</span><span class="sxs-lookup"><span data-stu-id="03774-375">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="03774-376">W tym samouczku następujące metody są wywoływane w kolekcji:</span><span class="sxs-lookup"><span data-stu-id="03774-376">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="03774-377">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; usuwa pojedynczy dokument pasujący do podanych kryteriów wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="03774-377">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="03774-378">[Znajdź\<TDocument >](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; zwraca wszystkie dokumenty w kolekcji pasujące do podanych kryteriów wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="03774-378">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="03774-379">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Wstawia podany obiekt jako nowy dokument w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="03774-379">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="03774-380">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; zastępuje pojedynczy dokument pasujący do podanych kryteriów wyszukiwania z podanym obiektem.</span><span class="sxs-lookup"><span data-stu-id="03774-380">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="03774-381">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="03774-381">Add a controller</span></span>

<span data-ttu-id="03774-382">Dodaj klasę `BooksController` do katalogu *controllers* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="03774-382">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Controllers/BooksController.cs)]

<span data-ttu-id="03774-383">Poprzedni kontroler interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="03774-383">The preceding web API controller:</span></span>

* <span data-ttu-id="03774-384">Używa klasy `BookService` do wykonywania operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="03774-384">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="03774-385">Zawiera metody akcji do obsługi żądań HTTP GET, POST, PUT i DELETE.</span><span class="sxs-lookup"><span data-stu-id="03774-385">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="03774-386">Wywołuje <xref:System.Web.Http.ApiController.CreatedAtRoute*> w metodzie `Create`, aby zwrócić odpowiedź [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) .</span><span class="sxs-lookup"><span data-stu-id="03774-386">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="03774-387">Kod stanu 201 jest standardową odpowiedzią dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="03774-387">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="03774-388">`CreatedAtRoute` dodaje również nagłówek `Location` do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="03774-388">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="03774-389">Nagłówek `Location` określa identyfikator URI nowo utworzonej książki.</span><span class="sxs-lookup"><span data-stu-id="03774-389">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="03774-390">Testowanie interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="03774-390">Test the web API</span></span>

1. <span data-ttu-id="03774-391">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="03774-391">Build and run the app.</span></span>

1. <span data-ttu-id="03774-392">Przejdź do `http://localhost:<port>/api/books`, aby przetestować bezparametryczne `Get` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="03774-392">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="03774-393">Zostanie wyświetlona następująca odpowiedź JSON:</span><span class="sxs-lookup"><span data-stu-id="03774-393">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="03774-394">Przejdź do `http://localhost:<port>/api/books/{id here}`, aby przetestować metodę `Get` akcji przeciążonej kontrolera.</span><span class="sxs-lookup"><span data-stu-id="03774-394">Navigate to `http://localhost:<port>/api/books/{id here}` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="03774-395">Zostanie wyświetlona następująca odpowiedź JSON:</span><span class="sxs-lookup"><span data-stu-id="03774-395">The following JSON response is displayed:</span></span>

   ```json
   {
     "id":"{ID}",
     "bookName":"Clean Code",
     "price":43.15,
     "category":"Computers",
     "author":"Robert C. Martin"
   }
   ```

## <a name="configure-json-serialization-options"></a><span data-ttu-id="03774-396">Konfigurowanie opcji serializacji JSON</span><span class="sxs-lookup"><span data-stu-id="03774-396">Configure JSON serialization options</span></span>

<span data-ttu-id="03774-397">Istnieją dwa szczegóły dotyczące odpowiedzi JSON zwróconych w sekcji [testowanie interfejsu API sieci Web](#test-the-web-api) :</span><span class="sxs-lookup"><span data-stu-id="03774-397">There are two details to change about the JSON responses returned in the [Test the web API](#test-the-web-api) section:</span></span>

* <span data-ttu-id="03774-398">Nazwy właściwości "default notacji CamelCase wielkość liter należy zmienić tak, aby pasowały do wielkości liter w języku Pascal nazw właściwości obiektu CLR.</span><span class="sxs-lookup"><span data-stu-id="03774-398">The property names' default camel casing should be changed to match the Pascal casing of the CLR object's property names.</span></span>
* <span data-ttu-id="03774-399">Właściwość `bookName` powinna zostać zwrócona jako `Name`.</span><span class="sxs-lookup"><span data-stu-id="03774-399">The `bookName` property should be returned as `Name`.</span></span>

<span data-ttu-id="03774-400">Aby spełnić powyższe wymagania, należy wprowadzić następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="03774-400">To satisfy the preceding requirements, make the following changes:</span></span>

1. <span data-ttu-id="03774-401">W `Startup.ConfigureServices`łańcuchować następujący wyróżniony kod do wywołania metody `AddMvc`:</span><span class="sxs-lookup"><span data-stu-id="03774-401">In `Startup.ConfigureServices`, chain the following highlighted code on to the `AddMvc` method call:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

   <span data-ttu-id="03774-402">W przypadku poprzedniej zmiany nazwy właściwości w serializowanej odpowiedzi JSON interfejsu API sieci Web pasują do odpowiednich nazw właściwości w typie obiektu CLR.</span><span class="sxs-lookup"><span data-stu-id="03774-402">With the preceding change, property names in the web API's serialized JSON response match their corresponding property names in the CLR object type.</span></span> <span data-ttu-id="03774-403">Na przykład Serializacja właściwości `Author` klasy `Book` jako `Author`.</span><span class="sxs-lookup"><span data-stu-id="03774-403">For example, the `Book` class's `Author` property serializes as `Author`.</span></span>

1. <span data-ttu-id="03774-404">W *modelach/książka. cs*Dodaj adnotację do właściwości `BookName` z następującym atrybutem [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) :</span><span class="sxs-lookup"><span data-stu-id="03774-404">In *Models/Book.cs*, annotate the `BookName` property with the following [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) attribute:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

   <span data-ttu-id="03774-405">Wartość `[JsonProperty]` atrybutu `Name` reprezentuje nazwę właściwości w serializowanej odpowiedzi JSON internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="03774-405">The `[JsonProperty]` attribute's value of `Name` represents the property name in the web API's serialized JSON response.</span></span>

1. <span data-ttu-id="03774-406">Dodaj następujący kod na górze *modeli/książek. cs* , aby rozwiązać `[JsonProperty]` odwołanie do atrybutu:</span><span class="sxs-lookup"><span data-stu-id="03774-406">Add the following code to the top of *Models/Book.cs* to resolve the `[JsonProperty]` attribute reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. <span data-ttu-id="03774-407">Powtórz kroki zdefiniowane w sekcji [testowanie interfejsu API sieci Web](#test-the-web-api) .</span><span class="sxs-lookup"><span data-stu-id="03774-407">Repeat the steps defined in the [Test the web API](#test-the-web-api) section.</span></span> <span data-ttu-id="03774-408">Zwróć uwagę na różnice w nazwach właściwości JSON.</span><span class="sxs-lookup"><span data-stu-id="03774-408">Notice the difference in JSON property names.</span></span>

::: moniker-end

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="03774-409">Dodawanie obsługi uwierzytelniania do internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="03774-409">Add authentication support to a web API</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="next-steps"></a><span data-ttu-id="03774-410">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="03774-410">Next steps</span></span>

<span data-ttu-id="03774-411">Aby uzyskać więcej informacji na temat tworzenia ASP.NET Core interfejsów API sieci Web, zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="03774-411">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="03774-412">Wersja tego artykułu usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="03774-412">YouTube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
