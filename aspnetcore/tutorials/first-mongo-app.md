---
title: Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i usługi MongoDB
author: prkhandelwal
description: W tym samouczku przedstawiono sposób tworzenia ASP.NET Core internetowego interfejsu API przy użyciu bazy danych NoSQL MongoDB.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc, seodec18
ms.date: 08/17/2019
uid: tutorials/first-mongo-app
ms.openlocfilehash: acf2ded8b92a8f77678af7b772ac2a69264a642c
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082375"
---
# <a name="create-a-web-api-with-aspnet-core-and-mongodb"></a><span data-ttu-id="a1c21-103">Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i usługi MongoDB</span><span class="sxs-lookup"><span data-stu-id="a1c21-103">Create a web API with ASP.NET Core and MongoDB</span></span>

<span data-ttu-id="a1c21-104">Przez [Pratik Khandelwal](https://twitter.com/K2Prk) i [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="a1c21-104">By [Pratik Khandelwal](https://twitter.com/K2Prk) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a1c21-105">Ten samouczek tworzy internetowego interfejsu API, która wykonuje operacje tworzenia, odczytu, aktualizowania lub usuwania (CRUD) [bazy danych MongoDB](https://www.mongodb.com/what-is-mongodb) bazy danych NoSQL.</span><span class="sxs-lookup"><span data-stu-id="a1c21-105">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="a1c21-106">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="a1c21-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a1c21-107">Konfigurowanie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="a1c21-107">Configure MongoDB</span></span>
> * <span data-ttu-id="a1c21-108">Tworzenie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="a1c21-108">Create a MongoDB database</span></span>
> * <span data-ttu-id="a1c21-109">Definiowanie kolekcji usługi MongoDB i schematu</span><span class="sxs-lookup"><span data-stu-id="a1c21-109">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="a1c21-110">Wykonywanie operacji CRUD bazy danych MongoDB z internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="a1c21-110">Perform MongoDB CRUD operations from a web API</span></span>
> * <span data-ttu-id="a1c21-111">Dostosowywanie serializacji JSON</span><span class="sxs-lookup"><span data-stu-id="a1c21-111">Customize JSON serialization</span></span>

<span data-ttu-id="a1c21-112">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a1c21-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a1c21-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="a1c21-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a1c21-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a1c21-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="a1c21-115">Zestaw .NET Core SDK 3,0 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a1c21-115">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="a1c21-116">[Program Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z **ASP.NET i programowaniem aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="a1c21-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="a1c21-117">MongoDB</span><span class="sxs-lookup"><span data-stu-id="a1c21-117">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a1c21-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a1c21-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="a1c21-119">Zestaw .NET Core SDK 3,0 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a1c21-119">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="a1c21-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a1c21-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="a1c21-121">Środowisko C# dla programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a1c21-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="a1c21-122">MongoDB</span><span class="sxs-lookup"><span data-stu-id="a1c21-122">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a1c21-123">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a1c21-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="a1c21-124">Zestaw .NET Core SDK 3,0 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a1c21-124">.NET Core SDK 3.0 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="a1c21-125">Program Visual Studio dla komputerów Mac w wersji 7,7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a1c21-125">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="a1c21-126">MongoDB</span><span class="sxs-lookup"><span data-stu-id="a1c21-126">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="a1c21-127">Konfigurowanie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="a1c21-127">Configure MongoDB</span></span>

<span data-ttu-id="a1c21-128">W przypadku korzystania z systemu Windows MongoDB jest instalowany w *C\\:\\Program Files MongoDB* domyślnie.</span><span class="sxs-lookup"><span data-stu-id="a1c21-128">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="a1c21-129">Dodaj *plik C\\: program\\Files\\MongoDB\\Serverversion_number\<>\\bin* do`Path` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="a1c21-129">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="a1c21-130">Dzięki tej zmianie bazy danych MongoDB dostęp z dowolnego miejsca na komputerze deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="a1c21-130">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="a1c21-131">Użyj powłoki mongo w poniższych krokach umożliwia tworzenie bazy danych, wprowadzanie kolekcje i przechowywanie dokumentów.</span><span class="sxs-lookup"><span data-stu-id="a1c21-131">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="a1c21-132">Aby uzyskać więcej informacji na temat poleceń powłoki mongo, zobacz [Praca z powłoki mongo](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="a1c21-132">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="a1c21-133">Wybierz katalog na komputerze deweloperskim do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="a1c21-133">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="a1c21-134">Na przykład *\\C: BooksData* w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="a1c21-134">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="a1c21-135">Utwórz katalog, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="a1c21-135">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="a1c21-136">Powłoki mongo nie tworzyć nowe katalogi.</span><span class="sxs-lookup"><span data-stu-id="a1c21-136">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="a1c21-137">Otwórz powłokę wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="a1c21-137">Open a command shell.</span></span> <span data-ttu-id="a1c21-138">Uruchom następujące polecenie, aby nawiązać połączenie z bazą danych MongoDB na domyślnym porcie 27017.</span><span class="sxs-lookup"><span data-stu-id="a1c21-138">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="a1c21-139">Pamiętaj, aby zastąpić `<data_directory_path>` z katalogu, który został wybrany w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="a1c21-139">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

   ```console
   mongod --dbpath <data_directory_path>
   ```

1. <span data-ttu-id="a1c21-140">Otwórz inne wystąpienie powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="a1c21-140">Open another command shell instance.</span></span> <span data-ttu-id="a1c21-141">Połączenia z bazą danych testu domyślną, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="a1c21-141">Connect to the default test database by running the following command:</span></span>

   ```console
   mongo
   ```

1. <span data-ttu-id="a1c21-142">Uruchom następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="a1c21-142">Run the following in a command shell:</span></span>

   ```console
   use BookstoreDb
   ```

   <span data-ttu-id="a1c21-143">Jeśli jeszcze nie istnieje, bazę danych o nazwie *BookstoreDb* zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="a1c21-143">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="a1c21-144">Jeśli baza danych istnieje, jego połączenie jest otwarte dla transakcji.</span><span class="sxs-lookup"><span data-stu-id="a1c21-144">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="a1c21-145">Utwórz `Books` kolekcji za pomocą następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="a1c21-145">Create a `Books` collection using following command:</span></span>

   ```console
   db.createCollection('Books')
   ```

   <span data-ttu-id="a1c21-146">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="a1c21-146">The following result is displayed:</span></span>

   ```console
   { "ok" : 1 }
   ```

1. <span data-ttu-id="a1c21-147">Zdefiniować schemat `Books` kolekcji i Wstaw dwa dokumenty, używając następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="a1c21-147">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

   ```console
   db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
   ```

   <span data-ttu-id="a1c21-148">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="a1c21-148">The following result is displayed:</span></span>

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
   > <span data-ttu-id="a1c21-149">Identyfikator przedstawiony w tym artykule nie będzie pasował do identyfikatorów podczas uruchamiania tego przykładu.</span><span class="sxs-lookup"><span data-stu-id="a1c21-149">The ID's shown in this article will not match the IDs when you run this sample.</span></span>

1. <span data-ttu-id="a1c21-150">Wyświetl dokumenty w bazie danych, używając następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="a1c21-150">View the documents in the database using the following command:</span></span>

   ```console
   db.Books.find({}).pretty()
   ```

   <span data-ttu-id="a1c21-151">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="a1c21-151">The following result is displayed:</span></span>

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

   <span data-ttu-id="a1c21-152">Schemat dodaje wygenerowany automatycznie `_id` właściwości typu `ObjectId` dla każdego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a1c21-152">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="a1c21-153">Baza danych jest gotowy.</span><span class="sxs-lookup"><span data-stu-id="a1c21-153">The database is ready.</span></span> <span data-ttu-id="a1c21-154">Możesz rozpocząć tworzenie interfejsu API sieci web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a1c21-154">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="a1c21-155">Utwórz projekt interfejsu API sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a1c21-155">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a1c21-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a1c21-156">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a1c21-157">Przejdź do pozycji **plik** > **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-157">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="a1c21-158">Wybierz typ projektu **aplikacja sieci Web ASP.NET Core** a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-158">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="a1c21-159">Nazwij projekt *BooksApi*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-159">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="a1c21-160">Wybierz platformę docelową **.NET Core** i **ASP.NET Core 3,0**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-160">Select the **.NET Core** target framework and **ASP.NET Core 3.0**.</span></span> <span data-ttu-id="a1c21-161">Wybierz szablon projektu **interfejsu API** i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-161">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="a1c21-162">Odwiedź galerię [NuGet: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) , aby określić najnowszą stabilną wersję sterownika .NET dla usługi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a1c21-162">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="a1c21-163">W **Konsola Menedżera pakietów** okna, przejdź do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="a1c21-163">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="a1c21-164">Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:</span><span class="sxs-lookup"><span data-stu-id="a1c21-164">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```powershell
   Install-Package MongoDB.Driver -Version {VERSION}
   ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a1c21-165">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a1c21-165">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="a1c21-166">Uruchom następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="a1c21-166">Run the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new webapi -o BooksApi
   code BooksApi
   ```

   <span data-ttu-id="a1c21-167">Nowy projekt interfejsu API sieci web platformy ASP.NET Core, na przeznaczonych dla platformy .NET Core jest wygenerowany i otworzyć w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a1c21-167">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="a1c21-168">Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w **oknie dialogowym zostanie wyświetlony monit o podanie wymaganych zasobów do skompilowania i debugowania z elementu "BooksApi". Dodać je?** .</span><span class="sxs-lookup"><span data-stu-id="a1c21-168">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="a1c21-169">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-169">Select **Yes**.</span></span>
1. <span data-ttu-id="a1c21-170">Odwiedź galerię [NuGet: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) , aby określić najnowszą stabilną wersję sterownika .NET dla usługi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a1c21-170">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="a1c21-171">Otwórz **zintegrowany Terminal** i przejdź do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="a1c21-171">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="a1c21-172">Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:</span><span class="sxs-lookup"><span data-stu-id="a1c21-172">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```dotnetcli
   dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a1c21-173">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a1c21-173">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="a1c21-174">Przejdź do pozycji **plik** > **nowe rozwiązanie** > **.NET Core** > **.**</span><span class="sxs-lookup"><span data-stu-id="a1c21-174">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="a1c21-175">Wybierz szablon projektu C# **interfejsu API sieci Web ASP.NET Core** i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-175">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="a1c21-176">Z listy rozwijanej **platforma docelowa** wybierz pozycję **.NET Core 3,0** , a następnie wybierz pozycję **Next (dalej**).</span><span class="sxs-lookup"><span data-stu-id="a1c21-176">Select **.NET Core 3.0** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="a1c21-177">Wprowadź *BooksApi* jako **nazwę projektu**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-177">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="a1c21-178">W **rozwiązania** konsoli kliknij prawym przyciskiem myszy projekt **zależności** a następnie wybierz węzeł **Dodawanie pakietów**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-178">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="a1c21-179">Wprowadź *MongoDB. Driver* w polu wyszukiwania, wybierz pakiet *MongoDB. Driver* , a następnie wybierz pozycję **Dodaj pakiet**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-179">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="a1c21-180">Wybierz przycisk **Akceptuj** w oknie dialogowym **akceptacji licencji** .</span><span class="sxs-lookup"><span data-stu-id="a1c21-180">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="a1c21-181">Dodaj model jednostki</span><span class="sxs-lookup"><span data-stu-id="a1c21-181">Add an entity model</span></span>

1. <span data-ttu-id="a1c21-182">Dodaj *modeli* katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="a1c21-182">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="a1c21-183">Dodaj `Book` klasy *modeli* katalogu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a1c21-183">Add a `Book` class to the *Models* directory with the following code:</span></span>

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

   <span data-ttu-id="a1c21-184">W poprzedniej klasie `Id` Właściwość:</span><span class="sxs-lookup"><span data-stu-id="a1c21-184">In the preceding class, the `Id` property:</span></span>

   * <span data-ttu-id="a1c21-185">Jest wymagany do mapowania obiektu środowiska uruchomieniowego języka wspólnego (CLR) do kolekcji MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a1c21-185">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
   * <span data-ttu-id="a1c21-186">Zawiera adnotację z [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) , aby wyznaczyć tę właściwość jako klucz podstawowy dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a1c21-186">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
   * <span data-ttu-id="a1c21-187">Zawiera adnotację z [[BsonRepresentation (BsonType. objectid)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) , aby zezwolić na przekazywanie parametru jako `string` typ zamiast struktury [objectid](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) .</span><span class="sxs-lookup"><span data-stu-id="a1c21-187">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="a1c21-188">Mongo obsługuje konwersję z `string` do `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="a1c21-188">Mongo handles the conversion from `string` to `ObjectId`.</span></span>

   <span data-ttu-id="a1c21-189">Właściwość ma adnotację z atrybutem [[BsonElement].](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) `BookName`</span><span class="sxs-lookup"><span data-stu-id="a1c21-189">The `BookName` property is annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="a1c21-190">Wartość `Name` atrybutu reprezentuje nazwę właściwości w kolekcji MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a1c21-190">The attribute's value of `Name` represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="a1c21-191">Dodaj model konfiguracji</span><span class="sxs-lookup"><span data-stu-id="a1c21-191">Add a configuration model</span></span>

1. <span data-ttu-id="a1c21-192">Dodaj następujące wartości konfiguracji bazy danych do pliku *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="a1c21-192">Add the following database configuration values to *appsettings.json*:</span></span>

   [!code-json[](first-mongo-app/samples/3.x/SampleApp/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="a1c21-193">Dodaj plik *BookstoreDatabaseSettings.cs* do katalogu *models* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="a1c21-193">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/BookstoreDatabaseSettings.cs)]

   <span data-ttu-id="a1c21-194">Powyższa `BookstoreDatabaseSettings` Klasa jest używana do przechowywania wartości `BookstoreDatabaseSettings` właściwości *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="a1c21-194">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="a1c21-195">Nazwy JSON i C# właściwości są nazywane identycznie, aby uprościć proces mapowania.</span><span class="sxs-lookup"><span data-stu-id="a1c21-195">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="a1c21-196">Dodaj następujący wyróżniony kod do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a1c21-196">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/3.x/SampleApp/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

   <span data-ttu-id="a1c21-197">W powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="a1c21-197">In the preceding code:</span></span>

   * <span data-ttu-id="a1c21-198">Wystąpienie konfiguracji, do którego są powiązane `BookstoreDatabaseSettings` sekcje pliku *appSettings. JSON* , jest zarejestrowane w kontenerze iniekcji zależności (di).</span><span class="sxs-lookup"><span data-stu-id="a1c21-198">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="a1c21-199">`BookstoreDatabaseSettings` Na przykład `ConnectionString`właściwośćobiektu jest wypełniana właściwościąwplikuappSettings.JSON.`BookstoreDatabaseSettings:ConnectionString`</span><span class="sxs-lookup"><span data-stu-id="a1c21-199">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
   * <span data-ttu-id="a1c21-200">Interfejs jest rejestrowany przy użyciu programu di z pojedynczym [okresem istnienia usługi.](xref:fundamentals/dependency-injection#service-lifetimes) `IBookstoreDatabaseSettings`</span><span class="sxs-lookup"><span data-stu-id="a1c21-200">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="a1c21-201">Po dowstrzykiwaniu wystąpienie interfejsu jest rozpoznawane `BookstoreDatabaseSettings` jako obiekt.</span><span class="sxs-lookup"><span data-stu-id="a1c21-201">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="a1c21-202">Dodaj następujący kod na początku *Startup.cs* , aby rozwiązać `BookstoreDatabaseSettings` odwołania i: `IBookstoreDatabaseSettings`</span><span class="sxs-lookup"><span data-stu-id="a1c21-202">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="a1c21-203">Dodawanie usługi operacji CRUD</span><span class="sxs-lookup"><span data-stu-id="a1c21-203">Add a CRUD operations service</span></span>

1. <span data-ttu-id="a1c21-204">Dodaj *usług* katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="a1c21-204">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="a1c21-205">Dodaj `BookService` klasy *usług* katalogu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a1c21-205">Add a `BookService` class to the *Services* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceClass)]

   <span data-ttu-id="a1c21-206">W poprzednim kodzie `IBookstoreDatabaseSettings` wystąpienie jest pobierane z funkcji di przez iniekcję konstruktora.</span><span class="sxs-lookup"><span data-stu-id="a1c21-206">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="a1c21-207">Ta technika zapewnia dostęp do wartości konfiguracyjnych *appSettings. JSON* , które zostały dodane w sekcji [Dodawanie modelu konfiguracji](#add-a-configuration-model) .</span><span class="sxs-lookup"><span data-stu-id="a1c21-207">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="a1c21-208">Dodaj następujący wyróżniony kod do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a1c21-208">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/3.x/SampleApp/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

   <span data-ttu-id="a1c21-209">W poprzednim kodzie `BookService` Klasa jest zarejestrowana przy użyciu funkcji di, aby obsługiwać iniekcję konstruktora w klasach zużywających.</span><span class="sxs-lookup"><span data-stu-id="a1c21-209">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="a1c21-210">Okres istnienia usługi pojedynczej jest najbardziej `BookService` odpowiedni, ponieważ pobiera bezpośrednią zależność od. `MongoClient`</span><span class="sxs-lookup"><span data-stu-id="a1c21-210">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="a1c21-211">Zgodnie z oficjalnymi `MongoClient` [wskazówkami dotyczącymi ponownego użycia klienta Mongo](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)należy zarejestrować się w programie di z pojedynczym okresem istnienia usługi.</span><span class="sxs-lookup"><span data-stu-id="a1c21-211">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="a1c21-212">Dodaj następujący kod na początku *Startup.cs* , aby rozwiązać `BookService` odwołanie:</span><span class="sxs-lookup"><span data-stu-id="a1c21-212">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="a1c21-213">`BookService` Klasa używa następujących `MongoDB.Driver` elementy członkowskie do wykonywania operacji CRUD w odniesieniu do bazy danych:</span><span class="sxs-lookup"><span data-stu-id="a1c21-213">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="a1c21-214">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Odczytuje wystąpienie serwera na potrzeby wykonywania operacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a1c21-214">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="a1c21-215">Konstruktor obiektu tej klasy znajduje się ciąg połączenia bazy danych MongoDB:</span><span class="sxs-lookup"><span data-stu-id="a1c21-215">The constructor of this class is provided the MongoDB connection string:</span></span>

  [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="a1c21-216">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Reprezentuje bazę danych Mongo na potrzeby wykonywania operacji.</span><span class="sxs-lookup"><span data-stu-id="a1c21-216">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="a1c21-217">W tym samouczku do uzyskiwania dostępu do danych w określonej kolekcji jest stosowana ogólna Metoda [getcollection\<TDocument > (Collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) .</span><span class="sxs-lookup"><span data-stu-id="a1c21-217">This tutorial uses the generic [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="a1c21-218">Wykonaj operacje CRUD w odniesieniu do kolekcji po wywołaniu tej metody.</span><span class="sxs-lookup"><span data-stu-id="a1c21-218">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="a1c21-219">W `GetCollection<TDocument>(collection)` wywołanie metody:</span><span class="sxs-lookup"><span data-stu-id="a1c21-219">In the `GetCollection<TDocument>(collection)` method call:</span></span>

  * <span data-ttu-id="a1c21-220">`collection` reprezentuje nazwę kolekcji.</span><span class="sxs-lookup"><span data-stu-id="a1c21-220">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="a1c21-221">`TDocument` reprezentuje typ obiektu CLR, które są przechowywane w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="a1c21-221">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="a1c21-222">`GetCollection<TDocument>(collection)`zwraca obiekt [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) reprezentujący kolekcję.</span><span class="sxs-lookup"><span data-stu-id="a1c21-222">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="a1c21-223">W tym samouczku następujące metody są wywoływane w kolekcji:</span><span class="sxs-lookup"><span data-stu-id="a1c21-223">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="a1c21-224">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Usuwa pojedynczy dokument pasujący do podanych kryteriów wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="a1c21-224">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="a1c21-225">[Znajdź\<TDocument >](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; zwraca wszystkie dokumenty w kolekcji pasujące do podanych kryteriów wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="a1c21-225">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="a1c21-226">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Wstawia podany obiekt jako nowy dokument w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="a1c21-226">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="a1c21-227">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Zamienia pojedynczy dokument pasujący do podanych kryteriów wyszukiwania z podanym obiektem.</span><span class="sxs-lookup"><span data-stu-id="a1c21-227">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="a1c21-228">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="a1c21-228">Add a controller</span></span>

<span data-ttu-id="a1c21-229">Dodaj `BooksController` klasy *kontrolerów* katalogu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a1c21-229">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Controllers/BooksController.cs)]

<span data-ttu-id="a1c21-230">Poprzedni kontroler internetowego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="a1c21-230">The preceding web API controller:</span></span>

* <span data-ttu-id="a1c21-231">Używa `BookService` klasy w celu wykonywania operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="a1c21-231">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="a1c21-232">Zawiera metody akcji w celu obsługi żądań GET, POST, PUT i DELETE protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a1c21-232">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="a1c21-233">Wywołuje <xref:System.Web.Http.ApiController.CreatedAtRoute*> metodę akcji `Create` , aby zwrócić odpowiedź [http 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) .</span><span class="sxs-lookup"><span data-stu-id="a1c21-233">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="a1c21-234">Kod stanu 201 jest standardową odpowiedzią dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="a1c21-234">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="a1c21-235">`CreatedAtRoute`dodaje `Location` również nagłówek do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a1c21-235">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="a1c21-236">`Location` Nagłówek określa identyfikator URI nowo utworzonej książki.</span><span class="sxs-lookup"><span data-stu-id="a1c21-236">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="a1c21-237">Testowanie interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="a1c21-237">Test the web API</span></span>

1. <span data-ttu-id="a1c21-238">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="a1c21-238">Build and run the app.</span></span>

1. <span data-ttu-id="a1c21-239">Przejdź do `http://localhost:<port>/api/books` w celu przetestowania metody `Get` akcji bez parametrów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a1c21-239">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="a1c21-240">Wyświetlane są następujące odpowiedź w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="a1c21-240">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="a1c21-241">Przejdź do `http://localhost:<port>/api/books/{id here}` , aby przetestować przeciążoną `Get` metodę działania kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a1c21-241">Navigate to `http://localhost:<port>/api/books/{id here}` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="a1c21-242">Wyświetlane są następujące odpowiedź w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="a1c21-242">The following JSON response is displayed:</span></span>

   ```json
   {
     "id":"{ID}",
     "bookName":"Clean Code",
     "price":43.15,
     "category":"Computers",
     "author":"Robert C. Martin"
   }
   ```

## <a name="configure-json-serialization-options"></a><span data-ttu-id="a1c21-243">Konfigurowanie opcji serializacji JSON</span><span class="sxs-lookup"><span data-stu-id="a1c21-243">Configure JSON serialization options</span></span>

<span data-ttu-id="a1c21-244">Istnieją dwa szczegóły dotyczące odpowiedzi JSON zwróconych w sekcji [testowanie interfejsu API sieci Web](#test-the-web-api) :</span><span class="sxs-lookup"><span data-stu-id="a1c21-244">There are two details to change about the JSON responses returned in the [Test the web API](#test-the-web-api) section:</span></span>

* <span data-ttu-id="a1c21-245">Nazwy właściwości "default notacji CamelCase wielkość liter należy zmienić tak, aby pasowały do wielkości liter w języku Pascal nazw właściwości obiektu CLR.</span><span class="sxs-lookup"><span data-stu-id="a1c21-245">The property names' default camel casing should be changed to match the Pascal casing of the CLR object's property names.</span></span>
* <span data-ttu-id="a1c21-246">Właściwość powinna zostać zwrócona jako `Name`. `bookName`</span><span class="sxs-lookup"><span data-stu-id="a1c21-246">The `bookName` property should be returned as `Name`.</span></span>

<span data-ttu-id="a1c21-247">Aby spełnić powyższe wymagania, należy wprowadzić następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="a1c21-247">To satisfy the preceding requirements, make the following changes:</span></span>

1. <span data-ttu-id="a1c21-248">JSON.NET został usunięty z ASP.NET udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="a1c21-248">JSON.NET has been removed from ASP.NET shared framework.</span></span> <span data-ttu-id="a1c21-249">Dodaj odwołanie do pakietu do [Microsoft. AspNetCore. MVC. NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson).</span><span class="sxs-lookup"><span data-stu-id="a1c21-249">Add a package reference to [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson).</span></span>

1. <span data-ttu-id="a1c21-250">W `Startup.ConfigureServices`programie łańcuchować następujący wyróżniony kod w `AddMvc` wywołaniu metody:</span><span class="sxs-lookup"><span data-stu-id="a1c21-250">In `Startup.ConfigureServices`, chain the following highlighted code on to the `AddMvc` method call:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

   <span data-ttu-id="a1c21-251">W przypadku poprzedniej zmiany nazwy właściwości w serializowanej odpowiedzi JSON interfejsu API sieci Web pasują do odpowiednich nazw właściwości w typie obiektu CLR.</span><span class="sxs-lookup"><span data-stu-id="a1c21-251">With the preceding change, property names in the web API's serialized JSON response match their corresponding property names in the CLR object type.</span></span> <span data-ttu-id="a1c21-252">Na przykład, `Book` Serializacja `Author` właściwości klasy jako `Author`.</span><span class="sxs-lookup"><span data-stu-id="a1c21-252">For example, the `Book` class's `Author` property serializes as `Author`.</span></span>

1. <span data-ttu-id="a1c21-253">W *modelach/książka. cs*Dodaj adnotację `BookName` do właściwości z następującym atrybutem [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) :</span><span class="sxs-lookup"><span data-stu-id="a1c21-253">In *Models/Book.cs*, annotate the `BookName` property with the following [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) attribute:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

   <span data-ttu-id="a1c21-254">`[JsonProperty]` WartośćatrybutureprezentujenazwęwłaściwościwserializowanejodpowiedziJSON`Name` interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a1c21-254">The `[JsonProperty]` attribute's value of `Name` represents the property name in the web API's serialized JSON response.</span></span>

1. <span data-ttu-id="a1c21-255">Dodaj następujący kod na górze *modeli/książek. cs* , aby rozwiązać `[JsonProperty]` odwołanie do atrybutu:</span><span class="sxs-lookup"><span data-stu-id="a1c21-255">Add the following code to the top of *Models/Book.cs* to resolve the `[JsonProperty]` attribute reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/3.x/SampleApp/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. <span data-ttu-id="a1c21-256">Powtórz kroki zdefiniowane w sekcji [testowanie interfejsu API sieci Web](#test-the-web-api) .</span><span class="sxs-lookup"><span data-stu-id="a1c21-256">Repeat the steps defined in the [Test the web API](#test-the-web-api) section.</span></span> <span data-ttu-id="a1c21-257">Zwróć uwagę na różnice w nazwach właściwości JSON.</span><span class="sxs-lookup"><span data-stu-id="a1c21-257">Notice the difference in JSON property names.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a1c21-258">Ten samouczek tworzy internetowego interfejsu API, która wykonuje operacje tworzenia, odczytu, aktualizowania lub usuwania (CRUD) [bazy danych MongoDB](https://www.mongodb.com/what-is-mongodb) bazy danych NoSQL.</span><span class="sxs-lookup"><span data-stu-id="a1c21-258">This tutorial creates a web API that performs Create, Read, Update, and Delete (CRUD) operations on a [MongoDB](https://www.mongodb.com/what-is-mongodb) NoSQL database.</span></span>

<span data-ttu-id="a1c21-259">Ten samouczek zawiera informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="a1c21-259">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a1c21-260">Konfigurowanie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="a1c21-260">Configure MongoDB</span></span>
> * <span data-ttu-id="a1c21-261">Tworzenie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="a1c21-261">Create a MongoDB database</span></span>
> * <span data-ttu-id="a1c21-262">Definiowanie kolekcji usługi MongoDB i schematu</span><span class="sxs-lookup"><span data-stu-id="a1c21-262">Define a MongoDB collection and schema</span></span>
> * <span data-ttu-id="a1c21-263">Wykonywanie operacji CRUD bazy danych MongoDB z internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="a1c21-263">Perform MongoDB CRUD operations from a web API</span></span>
> * <span data-ttu-id="a1c21-264">Dostosowywanie serializacji JSON</span><span class="sxs-lookup"><span data-stu-id="a1c21-264">Customize JSON serialization</span></span>

<span data-ttu-id="a1c21-265">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a1c21-265">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-mongo-app/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a1c21-266">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="a1c21-266">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a1c21-267">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a1c21-267">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="a1c21-268">.NET Core SDK 2.2</span><span class="sxs-lookup"><span data-stu-id="a1c21-268">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="a1c21-269">[Program Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z **ASP.NET i programowaniem aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="a1c21-269">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="a1c21-270">MongoDB</span><span class="sxs-lookup"><span data-stu-id="a1c21-270">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a1c21-271">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a1c21-271">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="a1c21-272">.NET Core SDK 2.2</span><span class="sxs-lookup"><span data-stu-id="a1c21-272">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="a1c21-273">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a1c21-273">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="a1c21-274">Środowisko C# dla programu Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a1c21-274">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="a1c21-275">MongoDB</span><span class="sxs-lookup"><span data-stu-id="a1c21-275">MongoDB</span></span>](https://docs.mongodb.com/manual/administration/install-community/)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a1c21-276">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a1c21-276">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* [<span data-ttu-id="a1c21-277">.NET Core SDK 2.2</span><span class="sxs-lookup"><span data-stu-id="a1c21-277">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="a1c21-278">Program Visual Studio dla komputerów Mac w wersji 7,7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="a1c21-278">Visual Studio for Mac version 7.7 or later</span></span>](https://visualstudio.microsoft.com/downloads/)
* [<span data-ttu-id="a1c21-279">MongoDB</span><span class="sxs-lookup"><span data-stu-id="a1c21-279">MongoDB</span></span>](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

---

## <a name="configure-mongodb"></a><span data-ttu-id="a1c21-280">Konfigurowanie bazy danych MongoDB</span><span class="sxs-lookup"><span data-stu-id="a1c21-280">Configure MongoDB</span></span>

<span data-ttu-id="a1c21-281">W przypadku korzystania z systemu Windows MongoDB jest instalowany w *C\\:\\Program Files MongoDB* domyślnie.</span><span class="sxs-lookup"><span data-stu-id="a1c21-281">If using Windows, MongoDB is installed at *C:\\Program Files\\MongoDB* by default.</span></span> <span data-ttu-id="a1c21-282">Dodaj *plik C\\: program\\Files\\MongoDB\\Serverversion_number\<>\\bin* do`Path` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="a1c21-282">Add *C:\\Program Files\\MongoDB\\Server\\\<version_number>\\bin* to the `Path` environment variable.</span></span> <span data-ttu-id="a1c21-283">Dzięki tej zmianie bazy danych MongoDB dostęp z dowolnego miejsca na komputerze deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="a1c21-283">This change enables MongoDB access from anywhere on your development machine.</span></span>

<span data-ttu-id="a1c21-284">Użyj powłoki mongo w poniższych krokach umożliwia tworzenie bazy danych, wprowadzanie kolekcje i przechowywanie dokumentów.</span><span class="sxs-lookup"><span data-stu-id="a1c21-284">Use the mongo Shell in the following steps to create a database, make collections, and store documents.</span></span> <span data-ttu-id="a1c21-285">Aby uzyskać więcej informacji na temat poleceń powłoki mongo, zobacz [Praca z powłoki mongo](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span><span class="sxs-lookup"><span data-stu-id="a1c21-285">For more information on mongo Shell commands, see [Working with the mongo Shell](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell).</span></span>

1. <span data-ttu-id="a1c21-286">Wybierz katalog na komputerze deweloperskim do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="a1c21-286">Choose a directory on your development machine for storing the data.</span></span> <span data-ttu-id="a1c21-287">Na przykład *\\C: BooksData* w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="a1c21-287">For example, *C:\\BooksData* on Windows.</span></span> <span data-ttu-id="a1c21-288">Utwórz katalog, jeśli nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="a1c21-288">Create the directory if it doesn't exist.</span></span> <span data-ttu-id="a1c21-289">Powłoki mongo nie tworzyć nowe katalogi.</span><span class="sxs-lookup"><span data-stu-id="a1c21-289">The mongo Shell doesn't create new directories.</span></span>
1. <span data-ttu-id="a1c21-290">Otwórz powłokę wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="a1c21-290">Open a command shell.</span></span> <span data-ttu-id="a1c21-291">Uruchom następujące polecenie, aby nawiązać połączenie z bazą danych MongoDB na domyślnym porcie 27017.</span><span class="sxs-lookup"><span data-stu-id="a1c21-291">Run the following command to connect to MongoDB on default port 27017.</span></span> <span data-ttu-id="a1c21-292">Pamiętaj, aby zastąpić `<data_directory_path>` z katalogu, który został wybrany w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="a1c21-292">Remember to replace `<data_directory_path>` with the directory you chose in the previous step.</span></span>

   ```console
   mongod --dbpath <data_directory_path>
   ```

1. <span data-ttu-id="a1c21-293">Otwórz inne wystąpienie powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="a1c21-293">Open another command shell instance.</span></span> <span data-ttu-id="a1c21-294">Połączenia z bazą danych testu domyślną, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="a1c21-294">Connect to the default test database by running the following command:</span></span>

   ```console
   mongo
   ```

1. <span data-ttu-id="a1c21-295">Uruchom następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="a1c21-295">Run the following in a command shell:</span></span>

   ```console
   use BookstoreDb
   ```

   <span data-ttu-id="a1c21-296">Jeśli jeszcze nie istnieje, bazę danych o nazwie *BookstoreDb* zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="a1c21-296">If it doesn't already exist, a database named *BookstoreDb* is created.</span></span> <span data-ttu-id="a1c21-297">Jeśli baza danych istnieje, jego połączenie jest otwarte dla transakcji.</span><span class="sxs-lookup"><span data-stu-id="a1c21-297">If the database does exist, its connection is opened for transactions.</span></span>

1. <span data-ttu-id="a1c21-298">Utwórz `Books` kolekcji za pomocą następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="a1c21-298">Create a `Books` collection using following command:</span></span>

   ```console
   db.createCollection('Books')
   ```

   <span data-ttu-id="a1c21-299">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="a1c21-299">The following result is displayed:</span></span>

   ```console
   { "ok" : 1 }
   ```

1. <span data-ttu-id="a1c21-300">Zdefiniować schemat `Books` kolekcji i Wstaw dwa dokumenty, używając następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="a1c21-300">Define a schema for the `Books` collection and insert two documents using the following command:</span></span>

   ```console
   db.Books.insertMany([{'Name':'Design Patterns','Price':54.93,'Category':'Computers','Author':'Ralph Johnson'}, {'Name':'Clean Code','Price':43.15,'Category':'Computers','Author':'Robert C. Martin'}])
   ```

   <span data-ttu-id="a1c21-301">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="a1c21-301">The following result is displayed:</span></span>

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
   > <span data-ttu-id="a1c21-302">Identyfikator przedstawiony w tym artykule nie będzie pasował do identyfikatorów podczas uruchamiania tego przykładu.</span><span class="sxs-lookup"><span data-stu-id="a1c21-302">The ID's shown in this article will not match the IDs when you run this sample.</span></span>

1. <span data-ttu-id="a1c21-303">Wyświetl dokumenty w bazie danych, używając następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="a1c21-303">View the documents in the database using the following command:</span></span>

   ```console
   db.Books.find({}).pretty()
   ```

   <span data-ttu-id="a1c21-304">Wyświetlane są następujące wyniki:</span><span class="sxs-lookup"><span data-stu-id="a1c21-304">The following result is displayed:</span></span>

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

   <span data-ttu-id="a1c21-305">Schemat dodaje wygenerowany automatycznie `_id` właściwości typu `ObjectId` dla każdego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a1c21-305">The schema adds an autogenerated `_id` property of type `ObjectId` for each document.</span></span>

<span data-ttu-id="a1c21-306">Baza danych jest gotowy.</span><span class="sxs-lookup"><span data-stu-id="a1c21-306">The database is ready.</span></span> <span data-ttu-id="a1c21-307">Możesz rozpocząć tworzenie interfejsu API sieci web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a1c21-307">You can start creating the ASP.NET Core web API.</span></span>

## <a name="create-the-aspnet-core-web-api-project"></a><span data-ttu-id="a1c21-308">Utwórz projekt interfejsu API sieci web platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a1c21-308">Create the ASP.NET Core web API project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a1c21-309">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a1c21-309">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a1c21-310">Przejdź do pozycji **plik** > **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-310">Go to **File** > **New** > **Project**.</span></span>
1. <span data-ttu-id="a1c21-311">Wybierz typ projektu **aplikacja sieci Web ASP.NET Core** a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-311">Select the **ASP.NET Core Web Application** project type, and select **Next**.</span></span>
1. <span data-ttu-id="a1c21-312">Nazwij projekt *BooksApi*, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-312">Name the project *BooksApi*, and select **Create**.</span></span>
1. <span data-ttu-id="a1c21-313">Wybierz platformę docelową **.NET Core** i **ASP.NET Core 2,2**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-313">Select the **.NET Core** target framework and **ASP.NET Core 2.2**.</span></span> <span data-ttu-id="a1c21-314">Wybierz szablon projektu **interfejsu API** i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-314">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="a1c21-315">Odwiedź galerię [NuGet: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) , aby określić najnowszą stabilną wersję sterownika .NET dla usługi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a1c21-315">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="a1c21-316">W **Konsola Menedżera pakietów** okna, przejdź do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="a1c21-316">In the **Package Manager Console** window, navigate to the project root.</span></span> <span data-ttu-id="a1c21-317">Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:</span><span class="sxs-lookup"><span data-stu-id="a1c21-317">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```powershell
   Install-Package MongoDB.Driver -Version {VERSION}
   ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a1c21-318">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a1c21-318">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="a1c21-319">Uruchom następujące polecenia w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="a1c21-319">Run the following commands in a command shell:</span></span>

   ```dotnetcli
   dotnet new webapi -o BooksApi
   code BooksApi
   ```

   <span data-ttu-id="a1c21-320">Nowy projekt interfejsu API sieci web platformy ASP.NET Core, na przeznaczonych dla platformy .NET Core jest wygenerowany i otworzyć w programie Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a1c21-320">A new ASP.NET Core web API project targeting .NET Core is generated and opened in Visual Studio Code.</span></span>

1. <span data-ttu-id="a1c21-321">Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w **oknie dialogowym zostanie wyświetlony monit o podanie wymaganych zasobów do skompilowania i debugowania z elementu "BooksApi". Dodać je?** .</span><span class="sxs-lookup"><span data-stu-id="a1c21-321">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'BooksApi'. Add them?**.</span></span> <span data-ttu-id="a1c21-322">Wybierz pozycję **tak**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-322">Select **Yes**.</span></span>
1. <span data-ttu-id="a1c21-323">Odwiedź galerię [NuGet: MongoDB. Driver](https://www.nuget.org/packages/MongoDB.Driver/) , aby określić najnowszą stabilną wersję sterownika .NET dla usługi MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a1c21-323">Visit the [NuGet Gallery: MongoDB.Driver](https://www.nuget.org/packages/MongoDB.Driver/) to determine the latest stable version of the .NET driver for MongoDB.</span></span> <span data-ttu-id="a1c21-324">Otwórz **zintegrowany Terminal** i przejdź do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="a1c21-324">Open **Integrated Terminal** and navigate to the project root.</span></span> <span data-ttu-id="a1c21-325">Uruchom następujące polecenie, aby zainstalować sterownik platformy .NET dla bazy danych MongoDB:</span><span class="sxs-lookup"><span data-stu-id="a1c21-325">Run the following command to install the .NET driver for MongoDB:</span></span>

   ```dotnetcli
   dotnet add BooksApi.csproj package MongoDB.Driver -v {VERSION}
   ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a1c21-326">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a1c21-326">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="a1c21-327">Przejdź do pozycji **plik** > **nowe rozwiązanie** > **.NET Core** > **.**</span><span class="sxs-lookup"><span data-stu-id="a1c21-327">Go to **File** > **New Solution** > **.NET Core** > **App**.</span></span>
1. <span data-ttu-id="a1c21-328">Wybierz szablon projektu C# **interfejsu API sieci Web ASP.NET Core** i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-328">Select the **ASP.NET Core Web API** C# project template, and select **Next**.</span></span>
1. <span data-ttu-id="a1c21-329">Z listy rozwijanej **platforma docelowa** wybierz pozycję **.NET Core 2,2** , a następnie wybierz pozycję **Next (dalej**).</span><span class="sxs-lookup"><span data-stu-id="a1c21-329">Select **.NET Core 2.2** from the **Target Framework** drop-down list, and select **Next**.</span></span>
1. <span data-ttu-id="a1c21-330">Wprowadź *BooksApi* jako **nazwę projektu**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-330">Enter *BooksApi* for the **Project Name**, and select **Create**.</span></span>
1. <span data-ttu-id="a1c21-331">W **rozwiązania** konsoli kliknij prawym przyciskiem myszy projekt **zależności** a następnie wybierz węzeł **Dodawanie pakietów**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-331">In the **Solution** pad, right-click the project's **Dependencies** node and select **Add Packages**.</span></span>
1. <span data-ttu-id="a1c21-332">Wprowadź *MongoDB. Driver* w polu wyszukiwania, wybierz pakiet *MongoDB. Driver* , a następnie wybierz pozycję **Dodaj pakiet**.</span><span class="sxs-lookup"><span data-stu-id="a1c21-332">Enter *MongoDB.Driver* in the search box, select the *MongoDB.Driver* package, and select **Add Package**.</span></span>
1. <span data-ttu-id="a1c21-333">Wybierz przycisk **Akceptuj** w oknie dialogowym **akceptacji licencji** .</span><span class="sxs-lookup"><span data-stu-id="a1c21-333">Select the **Accept** button in the **License Acceptance** dialog.</span></span>

---

## <a name="add-an-entity-model"></a><span data-ttu-id="a1c21-334">Dodaj model jednostki</span><span class="sxs-lookup"><span data-stu-id="a1c21-334">Add an entity model</span></span>

1. <span data-ttu-id="a1c21-335">Dodaj *modeli* katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="a1c21-335">Add a *Models* directory to the project root.</span></span>
1. <span data-ttu-id="a1c21-336">Dodaj `Book` klasy *modeli* katalogu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a1c21-336">Add a `Book` class to the *Models* directory with the following code:</span></span>

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

   <span data-ttu-id="a1c21-337">W poprzedniej klasie `Id` Właściwość:</span><span class="sxs-lookup"><span data-stu-id="a1c21-337">In the preceding class, the `Id` property:</span></span>

   * <span data-ttu-id="a1c21-338">Jest wymagany do mapowania obiektu środowiska uruchomieniowego języka wspólnego (CLR) do kolekcji MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a1c21-338">Is required for mapping the Common Language Runtime (CLR) object to the MongoDB collection.</span></span>
   * <span data-ttu-id="a1c21-339">Zawiera adnotację z [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) , aby wyznaczyć tę właściwość jako klucz podstawowy dokumentu.</span><span class="sxs-lookup"><span data-stu-id="a1c21-339">Is annotated with [[BsonId]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonIdAttribute.htm) to designate this property as the document's primary key.</span></span>
   * <span data-ttu-id="a1c21-340">Zawiera adnotację z [[BsonRepresentation (BsonType. objectid)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) , aby zezwolić na przekazywanie parametru jako `string` typ zamiast struktury [objectid](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) .</span><span class="sxs-lookup"><span data-stu-id="a1c21-340">Is annotated with [[BsonRepresentation(BsonType.ObjectId)]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonRepresentationAttribute.htm) to allow passing the parameter as type `string` instead of an [ObjectId](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_ObjectId.htm) structure.</span></span> <span data-ttu-id="a1c21-341">Mongo obsługuje konwersję z `string` do `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="a1c21-341">Mongo handles the conversion from `string` to `ObjectId`.</span></span>

   <span data-ttu-id="a1c21-342">Właściwość ma adnotację z atrybutem [[BsonElement].](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) `BookName`</span><span class="sxs-lookup"><span data-stu-id="a1c21-342">The `BookName` property is annotated with the [[BsonElement]](https://api.mongodb.com/csharp/current/html/T_MongoDB_Bson_Serialization_Attributes_BsonElementAttribute.htm) attribute.</span></span> <span data-ttu-id="a1c21-343">Wartość `Name` atrybutu reprezentuje nazwę właściwości w kolekcji MongoDB.</span><span class="sxs-lookup"><span data-stu-id="a1c21-343">The attribute's value of `Name` represents the property name in the MongoDB collection.</span></span>

## <a name="add-a-configuration-model"></a><span data-ttu-id="a1c21-344">Dodaj model konfiguracji</span><span class="sxs-lookup"><span data-stu-id="a1c21-344">Add a configuration model</span></span>

1. <span data-ttu-id="a1c21-345">Dodaj następujące wartości konfiguracji bazy danych do pliku *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="a1c21-345">Add the following database configuration values to *appsettings.json*:</span></span>

   [!code-json[](first-mongo-app/samples/2.x/SampleApp/appsettings.json?highlight=2-6)]

1. <span data-ttu-id="a1c21-346">Dodaj plik *BookstoreDatabaseSettings.cs* do katalogu *models* o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="a1c21-346">Add a *BookstoreDatabaseSettings.cs* file to the *Models* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/BookstoreDatabaseSettings.cs)]

   <span data-ttu-id="a1c21-347">Powyższa `BookstoreDatabaseSettings` Klasa jest używana do przechowywania wartości `BookstoreDatabaseSettings` właściwości *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="a1c21-347">The preceding `BookstoreDatabaseSettings` class is used to store the *appsettings.json* file's `BookstoreDatabaseSettings` property values.</span></span> <span data-ttu-id="a1c21-348">Nazwy JSON i C# właściwości są nazywane identycznie, aby uprościć proces mapowania.</span><span class="sxs-lookup"><span data-stu-id="a1c21-348">The JSON and C# property names are named identically to ease the mapping process.</span></span>

1. <span data-ttu-id="a1c21-349">Dodaj następujący wyróżniony kod do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a1c21-349">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/2.x/SampleApp/Startup.ConfigureServices.AddDbSettings.cs?highlight=3-7)]

   <span data-ttu-id="a1c21-350">W powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="a1c21-350">In the preceding code:</span></span>

   * <span data-ttu-id="a1c21-351">Wystąpienie konfiguracji, do którego są powiązane `BookstoreDatabaseSettings` sekcje pliku *appSettings. JSON* , jest zarejestrowane w kontenerze iniekcji zależności (di).</span><span class="sxs-lookup"><span data-stu-id="a1c21-351">The configuration instance to which the *appsettings.json* file's `BookstoreDatabaseSettings` section binds is registered in the Dependency Injection (DI) container.</span></span> <span data-ttu-id="a1c21-352">`BookstoreDatabaseSettings` Na przykład `ConnectionString`właściwośćobiektu jest wypełniana właściwościąwplikuappSettings.JSON.`BookstoreDatabaseSettings:ConnectionString`</span><span class="sxs-lookup"><span data-stu-id="a1c21-352">For example, a `BookstoreDatabaseSettings` object's `ConnectionString` property is populated with the `BookstoreDatabaseSettings:ConnectionString` property in *appsettings.json*.</span></span>
   * <span data-ttu-id="a1c21-353">Interfejs jest rejestrowany przy użyciu programu di z pojedynczym [okresem istnienia usługi.](xref:fundamentals/dependency-injection#service-lifetimes) `IBookstoreDatabaseSettings`</span><span class="sxs-lookup"><span data-stu-id="a1c21-353">The `IBookstoreDatabaseSettings` interface is registered in DI with a singleton [service lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="a1c21-354">Po dowstrzykiwaniu wystąpienie interfejsu jest rozpoznawane `BookstoreDatabaseSettings` jako obiekt.</span><span class="sxs-lookup"><span data-stu-id="a1c21-354">When injected, the interface instance resolves to a `BookstoreDatabaseSettings` object.</span></span>

1. <span data-ttu-id="a1c21-355">Dodaj następujący kod na początku *Startup.cs* , aby rozwiązać `BookstoreDatabaseSettings` odwołania i: `IBookstoreDatabaseSettings`</span><span class="sxs-lookup"><span data-stu-id="a1c21-355">Add the following code to the top of *Startup.cs* to resolve the `BookstoreDatabaseSettings` and `IBookstoreDatabaseSettings` references:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiModels)]

## <a name="add-a-crud-operations-service"></a><span data-ttu-id="a1c21-356">Dodawanie usługi operacji CRUD</span><span class="sxs-lookup"><span data-stu-id="a1c21-356">Add a CRUD operations service</span></span>

1. <span data-ttu-id="a1c21-357">Dodaj *usług* katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="a1c21-357">Add a *Services* directory to the project root.</span></span>
1. <span data-ttu-id="a1c21-358">Dodaj `BookService` klasy *usług* katalogu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a1c21-358">Add a `BookService` class to the *Services* directory with the following code:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceClass)]

   <span data-ttu-id="a1c21-359">W poprzednim kodzie `IBookstoreDatabaseSettings` wystąpienie jest pobierane z funkcji di przez iniekcję konstruktora.</span><span class="sxs-lookup"><span data-stu-id="a1c21-359">In the preceding code, an `IBookstoreDatabaseSettings` instance is retrieved from DI via constructor injection.</span></span> <span data-ttu-id="a1c21-360">Ta technika zapewnia dostęp do wartości konfiguracyjnych *appSettings. JSON* , które zostały dodane w sekcji [Dodawanie modelu konfiguracji](#add-a-configuration-model) .</span><span class="sxs-lookup"><span data-stu-id="a1c21-360">This technique provides access to the *appsettings.json* configuration values that were added in the [Add a configuration model](#add-a-configuration-model) section.</span></span>

1. <span data-ttu-id="a1c21-361">Dodaj następujący wyróżniony kod do `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a1c21-361">Add the following highlighted code to `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](first-mongo-app/samples_snapshot/2.x/SampleApp/Startup.ConfigureServices.AddSingletonService.cs?highlight=9)]

   <span data-ttu-id="a1c21-362">W poprzednim kodzie `BookService` Klasa jest zarejestrowana przy użyciu funkcji di, aby obsługiwać iniekcję konstruktora w klasach zużywających.</span><span class="sxs-lookup"><span data-stu-id="a1c21-362">In the preceding code, the `BookService` class is registered with DI to support constructor injection in consuming classes.</span></span> <span data-ttu-id="a1c21-363">Okres istnienia usługi pojedynczej jest najbardziej `BookService` odpowiedni, ponieważ pobiera bezpośrednią zależność od. `MongoClient`</span><span class="sxs-lookup"><span data-stu-id="a1c21-363">The singleton service lifetime is most appropriate because `BookService` takes a direct dependency on `MongoClient`.</span></span> <span data-ttu-id="a1c21-364">Zgodnie z oficjalnymi `MongoClient` [wskazówkami dotyczącymi ponownego użycia klienta Mongo](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use)należy zarejestrować się w programie di z pojedynczym okresem istnienia usługi.</span><span class="sxs-lookup"><span data-stu-id="a1c21-364">Per the official [Mongo Client reuse guidelines](https://mongodb.github.io/mongo-csharp-driver/2.8/reference/driver/connecting/#re-use), `MongoClient` should be registered in DI with a singleton service lifetime.</span></span>

1. <span data-ttu-id="a1c21-365">Dodaj następujący kod na początku *Startup.cs* , aby rozwiązać `BookService` odwołanie:</span><span class="sxs-lookup"><span data-stu-id="a1c21-365">Add the following code to the top of *Startup.cs* to resolve the `BookService` reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_UsingBooksApiServices)]

<span data-ttu-id="a1c21-366">`BookService` Klasa używa następujących `MongoDB.Driver` elementy członkowskie do wykonywania operacji CRUD w odniesieniu do bazy danych:</span><span class="sxs-lookup"><span data-stu-id="a1c21-366">The `BookService` class uses the following `MongoDB.Driver` members to perform CRUD operations against the database:</span></span>

* <span data-ttu-id="a1c21-367">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Odczytuje wystąpienie serwera na potrzeby wykonywania operacji bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a1c21-367">[MongoClient](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoClient.htm) &ndash; Reads the server instance for performing database operations.</span></span> <span data-ttu-id="a1c21-368">Konstruktor obiektu tej klasy znajduje się ciąg połączenia bazy danych MongoDB:</span><span class="sxs-lookup"><span data-stu-id="a1c21-368">The constructor of this class is provided the MongoDB connection string:</span></span>

  [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Services/BookService.cs?name=snippet_BookServiceConstructor&highlight=3)]

* <span data-ttu-id="a1c21-369">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Reprezentuje bazę danych Mongo na potrzeby wykonywania operacji.</span><span class="sxs-lookup"><span data-stu-id="a1c21-369">[IMongoDatabase](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_IMongoDatabase.htm) &ndash; Represents the Mongo database for performing operations.</span></span> <span data-ttu-id="a1c21-370">W tym samouczku do uzyskiwania dostępu do danych w określonej kolekcji jest stosowana ogólna Metoda [getcollection\<TDocument > (Collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) .</span><span class="sxs-lookup"><span data-stu-id="a1c21-370">This tutorial uses the generic [GetCollection\<TDocument>(collection)](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoDatabase_GetCollection__1.htm) method on the interface to gain access to data in a specific collection.</span></span> <span data-ttu-id="a1c21-371">Wykonaj operacje CRUD w odniesieniu do kolekcji po wywołaniu tej metody.</span><span class="sxs-lookup"><span data-stu-id="a1c21-371">Perform CRUD operations against the collection after this method is called.</span></span> <span data-ttu-id="a1c21-372">W `GetCollection<TDocument>(collection)` wywołanie metody:</span><span class="sxs-lookup"><span data-stu-id="a1c21-372">In the `GetCollection<TDocument>(collection)` method call:</span></span>

  * <span data-ttu-id="a1c21-373">`collection` reprezentuje nazwę kolekcji.</span><span class="sxs-lookup"><span data-stu-id="a1c21-373">`collection` represents the collection name.</span></span>
  * <span data-ttu-id="a1c21-374">`TDocument` reprezentuje typ obiektu CLR, które są przechowywane w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="a1c21-374">`TDocument` represents the CLR object type stored in the collection.</span></span>

<span data-ttu-id="a1c21-375">`GetCollection<TDocument>(collection)`zwraca obiekt [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) reprezentujący kolekcję.</span><span class="sxs-lookup"><span data-stu-id="a1c21-375">`GetCollection<TDocument>(collection)` returns a [MongoCollection](https://api.mongodb.com/csharp/current/html/T_MongoDB_Driver_MongoCollection.htm) object representing the collection.</span></span> <span data-ttu-id="a1c21-376">W tym samouczku następujące metody są wywoływane w kolekcji:</span><span class="sxs-lookup"><span data-stu-id="a1c21-376">In this tutorial, the following methods are invoked on the collection:</span></span>

* <span data-ttu-id="a1c21-377">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Usuwa pojedynczy dokument pasujący do podanych kryteriów wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="a1c21-377">[DeleteOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_DeleteOne.htm) &ndash; Deletes a single document matching the provided search criteria.</span></span>
* <span data-ttu-id="a1c21-378">[Znajdź\<TDocument >](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; zwraca wszystkie dokumenty w kolekcji pasujące do podanych kryteriów wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="a1c21-378">[Find\<TDocument>](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollectionExtensions_Find__1_1.htm) &ndash; Returns all documents in the collection matching the provided search criteria.</span></span>
* <span data-ttu-id="a1c21-379">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Wstawia podany obiekt jako nowy dokument w kolekcji.</span><span class="sxs-lookup"><span data-stu-id="a1c21-379">[InsertOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_InsertOne.htm) &ndash; Inserts the provided object as a new document in the collection.</span></span>
* <span data-ttu-id="a1c21-380">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Zamienia pojedynczy dokument pasujący do podanych kryteriów wyszukiwania z podanym obiektem.</span><span class="sxs-lookup"><span data-stu-id="a1c21-380">[ReplaceOne](https://api.mongodb.com/csharp/current/html/M_MongoDB_Driver_IMongoCollection_1_ReplaceOne.htm) &ndash; Replaces the single document matching the provided search criteria with the provided object.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="a1c21-381">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="a1c21-381">Add a controller</span></span>

<span data-ttu-id="a1c21-382">Dodaj `BooksController` klasy *kontrolerów* katalogu z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a1c21-382">Add a `BooksController` class to the *Controllers* directory with the following code:</span></span>

[!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Controllers/BooksController.cs)]

<span data-ttu-id="a1c21-383">Poprzedni kontroler internetowego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="a1c21-383">The preceding web API controller:</span></span>

* <span data-ttu-id="a1c21-384">Używa `BookService` klasy w celu wykonywania operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="a1c21-384">Uses the `BookService` class to perform CRUD operations.</span></span>
* <span data-ttu-id="a1c21-385">Zawiera metody akcji w celu obsługi żądań GET, POST, PUT i DELETE protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a1c21-385">Contains action methods to support GET, POST, PUT, and DELETE HTTP requests.</span></span>
* <span data-ttu-id="a1c21-386">Wywołuje <xref:System.Web.Http.ApiController.CreatedAtRoute*> metodę akcji `Create` , aby zwrócić odpowiedź [http 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) .</span><span class="sxs-lookup"><span data-stu-id="a1c21-386">Calls <xref:System.Web.Http.ApiController.CreatedAtRoute*> in the `Create` action method to return an [HTTP 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) response.</span></span> <span data-ttu-id="a1c21-387">Kod stanu 201 jest standardową odpowiedzią dla metody POST protokołu HTTP, która tworzy nowy zasób na serwerze.</span><span class="sxs-lookup"><span data-stu-id="a1c21-387">Status code 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="a1c21-388">`CreatedAtRoute`dodaje `Location` również nagłówek do odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a1c21-388">`CreatedAtRoute` also adds a `Location` header to the response.</span></span> <span data-ttu-id="a1c21-389">`Location` Nagłówek określa identyfikator URI nowo utworzonej książki.</span><span class="sxs-lookup"><span data-stu-id="a1c21-389">The `Location` header specifies the URI of the newly created book.</span></span>

## <a name="test-the-web-api"></a><span data-ttu-id="a1c21-390">Testowanie interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="a1c21-390">Test the web API</span></span>

1. <span data-ttu-id="a1c21-391">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="a1c21-391">Build and run the app.</span></span>

1. <span data-ttu-id="a1c21-392">Przejdź do `http://localhost:<port>/api/books` w celu przetestowania metody `Get` akcji bez parametrów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a1c21-392">Navigate to `http://localhost:<port>/api/books` to test the controller's parameterless `Get` action method.</span></span> <span data-ttu-id="a1c21-393">Wyświetlane są następujące odpowiedź w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="a1c21-393">The following JSON response is displayed:</span></span>

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

1. <span data-ttu-id="a1c21-394">Przejdź do `http://localhost:<port>/api/books/{id here}` , aby przetestować przeciążoną `Get` metodę działania kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a1c21-394">Navigate to `http://localhost:<port>/api/books/{id here}` to test the controller's overloaded `Get` action method.</span></span> <span data-ttu-id="a1c21-395">Wyświetlane są następujące odpowiedź w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="a1c21-395">The following JSON response is displayed:</span></span>

   ```json
   {
     "id":"{ID}",
     "bookName":"Clean Code",
     "price":43.15,
     "category":"Computers",
     "author":"Robert C. Martin"
   }
   ```

## <a name="configure-json-serialization-options"></a><span data-ttu-id="a1c21-396">Konfigurowanie opcji serializacji JSON</span><span class="sxs-lookup"><span data-stu-id="a1c21-396">Configure JSON serialization options</span></span>

<span data-ttu-id="a1c21-397">Istnieją dwa szczegóły dotyczące odpowiedzi JSON zwróconych w sekcji [testowanie interfejsu API sieci Web](#test-the-web-api) :</span><span class="sxs-lookup"><span data-stu-id="a1c21-397">There are two details to change about the JSON responses returned in the [Test the web API](#test-the-web-api) section:</span></span>

* <span data-ttu-id="a1c21-398">Nazwy właściwości "default notacji CamelCase wielkość liter należy zmienić tak, aby pasowały do wielkości liter w języku Pascal nazw właściwości obiektu CLR.</span><span class="sxs-lookup"><span data-stu-id="a1c21-398">The property names' default camel casing should be changed to match the Pascal casing of the CLR object's property names.</span></span>
* <span data-ttu-id="a1c21-399">Właściwość powinna zostać zwrócona jako `Name`. `bookName`</span><span class="sxs-lookup"><span data-stu-id="a1c21-399">The `bookName` property should be returned as `Name`.</span></span>

<span data-ttu-id="a1c21-400">Aby spełnić powyższe wymagania, należy wprowadzić następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="a1c21-400">To satisfy the preceding requirements, make the following changes:</span></span>

1. <span data-ttu-id="a1c21-401">W `Startup.ConfigureServices`programie łańcuchować następujący wyróżniony kod w `AddMvc` wywołaniu metody:</span><span class="sxs-lookup"><span data-stu-id="a1c21-401">In `Startup.ConfigureServices`, chain the following highlighted code on to the `AddMvc` method call:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Startup.cs?name=snippet_ConfigureServices&highlight=12)]

   <span data-ttu-id="a1c21-402">W przypadku poprzedniej zmiany nazwy właściwości w serializowanej odpowiedzi JSON interfejsu API sieci Web pasują do odpowiednich nazw właściwości w typie obiektu CLR.</span><span class="sxs-lookup"><span data-stu-id="a1c21-402">With the preceding change, property names in the web API's serialized JSON response match their corresponding property names in the CLR object type.</span></span> <span data-ttu-id="a1c21-403">Na przykład, `Book` Serializacja `Author` właściwości klasy jako `Author`.</span><span class="sxs-lookup"><span data-stu-id="a1c21-403">For example, the `Book` class's `Author` property serializes as `Author`.</span></span>

1. <span data-ttu-id="a1c21-404">W *modelach/książka. cs*Dodaj adnotację `BookName` do właściwości z następującym atrybutem [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) :</span><span class="sxs-lookup"><span data-stu-id="a1c21-404">In *Models/Book.cs*, annotate the `BookName` property with the following [[JsonProperty]](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_JsonPropertyAttribute.htm) attribute:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/Book.cs?name=snippet_BookNameProperty&highlight=2)]

   <span data-ttu-id="a1c21-405">`[JsonProperty]` WartośćatrybutureprezentujenazwęwłaściwościwserializowanejodpowiedziJSON`Name` interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a1c21-405">The `[JsonProperty]` attribute's value of `Name` represents the property name in the web API's serialized JSON response.</span></span>

1. <span data-ttu-id="a1c21-406">Dodaj następujący kod na górze *modeli/książek. cs* , aby rozwiązać `[JsonProperty]` odwołanie do atrybutu:</span><span class="sxs-lookup"><span data-stu-id="a1c21-406">Add the following code to the top of *Models/Book.cs* to resolve the `[JsonProperty]` attribute reference:</span></span>

   [!code-csharp[](first-mongo-app/samples/2.x/SampleApp/Models/Book.cs?name=snippet_NewtonsoftJsonImport)]

1. <span data-ttu-id="a1c21-407">Powtórz kroki zdefiniowane w sekcji [testowanie interfejsu API sieci Web](#test-the-web-api) .</span><span class="sxs-lookup"><span data-stu-id="a1c21-407">Repeat the steps defined in the [Test the web API](#test-the-web-api) section.</span></span> <span data-ttu-id="a1c21-408">Zwróć uwagę na różnice w nazwach właściwości JSON.</span><span class="sxs-lookup"><span data-stu-id="a1c21-408">Notice the difference in JSON property names.</span></span>

::: moniker-end

## <a name="next-steps"></a><span data-ttu-id="a1c21-409">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="a1c21-409">Next steps</span></span>

<span data-ttu-id="a1c21-410">Aby uzyskać więcej informacji dotyczących tworzenia interfejsów API sieci web platformy ASP.NET Core zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="a1c21-410">For more information on building ASP.NET Core web APIs, see the following resources:</span></span>

* [<span data-ttu-id="a1c21-411">Wersja tego artykułu usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="a1c21-411">YouTube version of this article</span></span>](https://www.youtube.com/watch?v=7uJt_sOenyo&feature=youtu.be)
* <xref:web-api/index>
* <xref:web-api/action-return-types>
