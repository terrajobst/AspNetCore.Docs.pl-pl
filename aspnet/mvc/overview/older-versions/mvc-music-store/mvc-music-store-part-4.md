---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Część 4: Modele i dostęp do danych | Dokumentacja firmy Microsoft'
author: jongalloway
description: Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 4 obejmuje modele i dostęp do danych.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 76671bbc7050d111b4d156c45584ba5aa4f1ea8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879481"
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="f408e-104">Część 4: Modele i dostęp do danych</span><span class="sxs-lookup"><span data-stu-id="f408e-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="f408e-105">przez [Galloway Jan](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="f408e-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="f408e-106">Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f408e-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="f408e-107">Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="f408e-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="f408e-108">Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu.</span><span class="sxs-lookup"><span data-stu-id="f408e-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="f408e-109">Część 4 obejmuje modele i dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="f408e-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="f408e-110">Do tej pory firma Microsoft został właśnie zostały przekazaniem "fikcyjny" z naszych kontrolerów do naszym szablony widoku.</span><span class="sxs-lookup"><span data-stu-id="f408e-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="f408e-111">Teraz już wszystko gotowe do Podłączanie rzeczywistych bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f408e-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="f408e-112">W tym samouczku firma Microsoft będzie obejmował jak używać programu SQL Server Compact Edition (często nazywane SQL CE) jako naszych aparatu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f408e-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="f408e-113">SQL CE jest bezpłatne, plik osadzony na podstawie bazy danych, która nie wymaga żadnych instalacji lub konfiguracji, która ułatwia naprawdę dla rozwoju lokalnych.</span><span class="sxs-lookup"><span data-stu-id="f408e-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="f408e-114">Dostęp do bazy danych z Entity Framework kod pierwszego</span><span class="sxs-lookup"><span data-stu-id="f408e-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="f408e-115">Użyjemy Obsługa Entity Framework (EF) znajduje się w projektach ASP.NET MVC 3 w celu wykonywania zapytań i zaktualizować bazę danych.</span><span class="sxs-lookup"><span data-stu-id="f408e-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="f408e-116">EF jest obiektem elastyczne relacyjne mapowanie danych (ORM) interfejsu API, który umożliwia deweloperom zapytań i aktualizacji danych przechowywanych w bazie danych w sposób zorientowane obiektowo.</span><span class="sxs-lookup"><span data-stu-id="f408e-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="f408e-117">Entity Framework w wersji 4 obsługuje modelu programowania, nazywany pierwszy kod.</span><span class="sxs-lookup"><span data-stu-id="f408e-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="f408e-118">Pierwszy kod służy do tworzenia obiektu modelu pisząc klasy proste (znanej także jako POCO z obiektów CLR "zwykły stary"), a może nawet utworzyć bazę danych na bieżąco z klas.</span><span class="sxs-lookup"><span data-stu-id="f408e-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="f408e-119">Zmiany w naszej klasy modelu</span><span class="sxs-lookup"><span data-stu-id="f408e-119">Changes to our Model Classes</span></span>

<span data-ttu-id="f408e-120">Firma Microsoft będzie można korzystania z funkcji tworzenia bazy danych w programie Entity Framework w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="f408e-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="f408e-121">Zanim przejdziemy, jednak upewnijmy kilka drobne zmiany do naszej klasy modelu mają zostać dodane w kilka rzeczy, który będzie używany na dalszym etapie.</span><span class="sxs-lookup"><span data-stu-id="f408e-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="f408e-122">Dodawanie klas modelu wykonawcy</span><span class="sxs-lookup"><span data-stu-id="f408e-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="f408e-123">Nasze albumów zostanie skojarzona z Artystami, więc dodamy klasę modelu proste do opisania wykonawcy.</span><span class="sxs-lookup"><span data-stu-id="f408e-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="f408e-124">Dodaj nową klasę do folderu modeli o nazwie Artist.cs przy użyciu kodu pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="f408e-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="f408e-125">Aktualizowanie naszej klasy modelu</span><span class="sxs-lookup"><span data-stu-id="f408e-125">Updating our Model Classes</span></span>

<span data-ttu-id="f408e-126">Klasa albumu aktualizacji, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="f408e-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="f408e-127">Następnie wprowadzić następujące aktualizacje do klasy Genre.</span><span class="sxs-lookup"><span data-stu-id="f408e-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="f408e-128">Dodawanie aplikacji\_folderu danych</span><span class="sxs-lookup"><span data-stu-id="f408e-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="f408e-129">Dodamy aplikacji\_katalog danych do naszej projektu do przechowywania plików bazy danych programu SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="f408e-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="f408e-130">Aplikacja\_dane są specjalne katalogu w programie ASP.NET, który ma już uprawnienia dostępu poprawnych zabezpieczeń dla dostępu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f408e-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="f408e-131">Wybierz z menu Projekt Dodaj Folder programu ASP.NET, a następnie aplikacji\_danych.</span><span class="sxs-lookup"><span data-stu-id="f408e-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="f408e-132">Tworzenie parametrów połączenia w pliku web.config</span><span class="sxs-lookup"><span data-stu-id="f408e-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="f408e-133">Dodamy kilka wierszy do pliku konfiguracji witryny sieci Web tak, aby Entity Framework potrafi nawiązać połączenia z naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f408e-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="f408e-134">Kliknij dwukrotnie plik Web.config znajduje się w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="f408e-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="f408e-135">Przewiń w dół ten plik i dodać &lt;connectionStrings&gt; sekcji bezpośrednio nad ostatni wiersz, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="f408e-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="f408e-136">Dodawanie klasy kontekstu</span><span class="sxs-lookup"><span data-stu-id="f408e-136">Adding a Context Class</span></span>

<span data-ttu-id="f408e-137">Kliknij prawym przyciskiem myszy folder modeli i Dodaj nową klasę o nazwie MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="f408e-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="f408e-138">Ta klasa zostanie reprezentują kontekst bazy danych programu Entity Framework i zostanie obsługi naszych Utwórz, odczytu aktualizacji i usuwanie operacji firmie Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f408e-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="f408e-139">Poniżej przedstawiono kod dla tej klasy.</span><span class="sxs-lookup"><span data-stu-id="f408e-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="f408e-140">To wszystko — nie żadnych innych konfiguracji specjalnych interfejsów, itp. Przez rozszerzenie klasy podstawowej typu DbContext, klasy Nasze MusicStoreEntities jest w stanie obsłużyć naszego operacje bazy danych w firmie Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f408e-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="f408e-141">Teraz, gdy mamy który podłączonymi Dodajmy kilka więcej właściwości do naszej klasy modeli, aby móc korzystać z niektórych dodatkowych informacji w naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f408e-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="f408e-142">Dodawanie magazynu danych katalogu</span><span class="sxs-lookup"><span data-stu-id="f408e-142">Adding our store catalog data</span></span>

<span data-ttu-id="f408e-143">Firma Microsoft będzie korzystać z funkcji w Entity Framework, która dodaje "seed" danych do nowo utworzonej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f408e-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="f408e-144">Spowoduje to wstępnego wypełniania naszego katalogu magazynu z listą Genres, artystów i albumów.</span><span class="sxs-lookup"><span data-stu-id="f408e-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="f408e-145">Pobieranie MvcMusicStore Assets.zip - zawierającego plików projektu lokacji używany we wcześniejszej części tego samouczka - ma plik klasy przy użyciu tych danych inicjatora, znajduje się w folderze o nazwie kodu.</span><span class="sxs-lookup"><span data-stu-id="f408e-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="f408e-146">W kodzie / folderu modeli, zlokalizuj plik SampleData.cs i upuść go w folderze modeli w naszym projektu, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="f408e-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="f408e-147">Teraz należy dodać jeden wiersz kodu powiedzieć o tej klasy SampleData Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f408e-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="f408e-148">Kliknij dwukrotnie plik Global.asax w folderze głównym projektu, aby go otworzyć i Dodaj poniższy wiersz do góry aplikacji\_Start — metoda.</span><span class="sxs-lookup"><span data-stu-id="f408e-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="f408e-149">W tym momencie została ukończona pracy niezbędne do skonfigurowania programu Entity Framework dla naszych projektu.</span><span class="sxs-lookup"><span data-stu-id="f408e-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="f408e-150">Zapytanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="f408e-150">Querying the Database</span></span>

<span data-ttu-id="f408e-151">Teraz załóżmy zaktualizować naszych StoreController tak, aby zamiast "fikcyjny danych" zamiast tego w naszej bazie danych do zapytania, wszystkie jej informacje.</span><span class="sxs-lookup"><span data-stu-id="f408e-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="f408e-152">Zaczniemy od zadeklarowania pola na **StoreController** do przechowywania wystąpienia klasy MusicStoreEntities o nazwie storeDB:</span><span class="sxs-lookup"><span data-stu-id="f408e-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="f408e-153">Aktualizowanie indeks magazynu w bazie danych</span><span class="sxs-lookup"><span data-stu-id="f408e-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="f408e-154">Klasa MusicStoreEntities jest obsługiwany przez program Entity Framework i udostępnia właściwości kolekcji dla każdej tabeli w naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f408e-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="f408e-155">Teraz zaktualizuj naszych StoreController akcji indeksu można pobrać wszystkich Genres w naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f408e-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="f408e-156">Wcześniej Robiliśmy to przez kodować danych ciągu.</span><span class="sxs-lookup"><span data-stu-id="f408e-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="f408e-157">Teraz możemy zamiast tego użyć kontekstu programu Entity Framework Generes kolekcji:</span><span class="sxs-lookup"><span data-stu-id="f408e-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="f408e-158">Brak zmian muszą się stać z naszych wyświetlanie szablonu, ponieważ firma Microsoft jest nadal zwracanie tego samego StoreIndexViewModel, możemy zwrócić przed - firma Microsoft jest po prostu zwracać dane na żywo z naszej bazie danych teraz.</span><span class="sxs-lookup"><span data-stu-id="f408e-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="f408e-159">Gdy firma Microsoft Uruchom ponownie projekt i odwiedź adres URL "/ przechowywania", firma Microsoft teraz wyświetlona lista wszystkich gatunkami muzyki w naszej bazie danych:</span><span class="sxs-lookup"><span data-stu-id="f408e-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="f408e-160">Aktualizowanie Przeglądaj magazynu i szczegóły korzystać z bieżących danych</span><span class="sxs-lookup"><span data-stu-id="f408e-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="f408e-161">Z/magazynu/Przeglądaj? genre =*[niektóre genre]* metody akcji, możemy poszukiwaną określonego rodzaju według nazwy.</span><span class="sxs-lookup"><span data-stu-id="f408e-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="f408e-162">Tylko oczekujemy jeden wynik, ponieważ firma Microsoft nigdy nie powinien zawierać dwa wpisy dla tej samej nazwie Genre, w związku z czym możemy użyć. Rozszerzenie Single() w składniku LINQ do zapytania dla odpowiedniego obiektu Genre następująco (nie należy wpisywać to jeszcze):</span><span class="sxs-lookup"><span data-stu-id="f408e-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="f408e-163">Pojedynczy metoda korzysta z wyrażenia Lambda jako parametr, który określa, że chcemy pojedynczy obiekt Genre taki sposób, że jego nazwa jest zgodna z wartością, już zdefiniowanego.</span><span class="sxs-lookup"><span data-stu-id="f408e-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="f408e-164">W przypadku powyższych możemy ładowania obiektu jednego rodzaju z wartością nazwy dopasowania Disco.</span><span class="sxs-lookup"><span data-stu-id="f408e-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="f408e-165">Firma Microsoft będzie korzystać z funkcji programu Entity Framework, umożliwiający wskazywać inne pokrewne jednostki, którą chcemy udostępnić również załadowany po pobraniu obiektu rodzaju.</span><span class="sxs-lookup"><span data-stu-id="f408e-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="f408e-166">Ta funkcja jest wywoływana kształtowania wynik zapytania i pozwala zmniejszyć liczbę razy, musimy dostęp do bazy danych, aby pobrać wszystkie potrzebne informacje.</span><span class="sxs-lookup"><span data-stu-id="f408e-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="f408e-167">Chcemy wstępnie pobrać albumów dla rodzaju Pobieramy, więc będziemy informować naszych zapytania do dołączenia z Genres.Include("Albums"), aby wskazać, że firma Microsoft ma również albumów powiązane.</span><span class="sxs-lookup"><span data-stu-id="f408e-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="f408e-168">Jest to bardziej wydajne, ponieważ będą pobierane dane zarówno naszych Genre, jak i albumu w żądaniu pojedynczej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f408e-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="f408e-169">Wraz z wyjaśnieniem przeszkadza poniżej przedstawiono wygląd naszego zaktualizowane akcji kontrolera przeglądania:</span><span class="sxs-lookup"><span data-stu-id="f408e-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="f408e-170">Mamy teraz zaktualizować widoku Przeglądaj magazynu, aby wyświetlić albumów, które są dostępne każdego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="f408e-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="f408e-171">Otwórz szablon widoku (znaleziony w /Views/Store/Browse.cshtml), a następnie dodać listy punktowane albumy, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="f408e-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="f408e-172">Z naszej aplikacji i przechodząc do sklepu/Przeglądaj? genre = Jazz wskazuje, że nasze wyniki są teraz pochodzi z bazy danych, wyświetlanie albumów wszystkich naszych wybranego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="f408e-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="f408e-173">Wybierzemy takie same, Zmień na naszych/Store/szczegółów lub [id] adres URL i Zastąp fikcyjny danych zapytania bazy danych, który jest ładowany albumu o identyfikatorze jest zgodna z wartością parametru.</span><span class="sxs-lookup"><span data-stu-id="f408e-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="f408e-174">Z naszej aplikacji i wyszukując /Store/Details/1 pokazuje, że nasze wyniki są teraz pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f408e-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="f408e-175">Teraz, kiedy naszą stronę Szczegóły magazynu jest skonfigurowane do wyświetlenia albumu według Identyfikatora albumy, możemy zaktualizować **Przeglądaj** widok, aby utworzyć łącze do widoku szczegółów.</span><span class="sxs-lookup"><span data-stu-id="f408e-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="f408e-176">Używamy Html.ActionLink, dokładnie tak, jak robiliśmy link z indeksem magazynu do magazynu, przejdź na końcu poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="f408e-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="f408e-177">Pełne źródło dla widoku przeglądania pojawia się poniżej.</span><span class="sxs-lookup"><span data-stu-id="f408e-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="f408e-178">Teraz możemy przejść z naszą stronę Sklepu do strony Genre, który znajduje się lista dostępnych albumy, i klikając albumu możemy wyświetlić szczegóły dotyczące danego albumu.</span><span class="sxs-lookup"><span data-stu-id="f408e-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="f408e-179">[Poprzednie](mvc-music-store-part-3.md)
> [dalej](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="f408e-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
