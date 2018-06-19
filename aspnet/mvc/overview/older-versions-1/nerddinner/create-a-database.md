---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Utwórz bazę danych | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 2 przedstawiono kroki, aby utworzyć bazę danych zawierający wszystkie obiad i RSVP danych dla aplikacji NerdDinner.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: ba28d671bf13ec54b83b876462e2c23f90310037
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869143"
---
<a name="create-a-database"></a><span data-ttu-id="d3424-103">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="d3424-103">Create a Database</span></span>
====================
<span data-ttu-id="d3424-104">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d3424-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="d3424-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="d3424-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="d3424-106">Jest to krok 2 z bezpłatny ["NerdDinner" samouczek aplikacji](introducing-the-nerddinner-tutorial.md) który przeszukiwań przez proces kompilacji mały, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="d3424-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="d3424-107">Krok 2 przedstawiono kroki, aby utworzyć bazę danych zawierający wszystkie obiad i RSVP danych dla aplikacji NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="d3424-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="d3424-108">Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.</span><span class="sxs-lookup"><span data-stu-id="d3424-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="d3424-109">NerdDinner krok 2: Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="d3424-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="d3424-110">Bazy danych będzie używany do przechowywania wszystkich danych obiad i RSVP w naszej aplikacji NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="d3424-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="d3424-111">W poniższych krokach przedstawiono tworzenie bazy danych przy użyciu bezpłatna wersja programu SQL Server Express (które można łatwo zainstalować przy użyciu V2 z [Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="d3424-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="d3424-112">Cały kod przedstawiono tworzenie firma Microsoft współpracuje z programu SQL Server Express i pełnego serwera SQL.</span><span class="sxs-lookup"><span data-stu-id="d3424-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="d3424-113">Tworzenie nowej bazy danych programu SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="d3424-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="d3424-114">Firma Microsoft będzie rozpocząć, klikając prawym przyciskiem myszy na naszych projektu sieci web, a następnie wybierz **Add -&gt;nowy element** polecenie:</span><span class="sxs-lookup"><span data-stu-id="d3424-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="d3424-115">Zostanie wyświetlone okno dialogowe "Dodaj nowy element" programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3424-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="d3424-116">Firma Microsoft będzie filtrować według kategorii "Dane" i wybierz szablon elementu "Bazy danych SQL Server":</span><span class="sxs-lookup"><span data-stu-id="d3424-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="d3424-117">Firma Microsoft będzie nazwa programu SQL Server Express bazy danych, którą chcemy, aby utworzyć "NerdDinner.mdf" i kliknij przycisk ok.</span><span class="sxs-lookup"><span data-stu-id="d3424-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="d3424-118">Visual Studio zostanie następnie poprosić nam Jeśli chcemy dodać ten plik do naszej \App\_katalog danych (która jest już katalog instalacja przy użyciu odczytu i zapisu zabezpieczeń listy ACL):</span><span class="sxs-lookup"><span data-stu-id="d3424-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="d3424-119">Firma Microsoft będzie kliknij przycisk "Tak" i naszej nowej bazy danych zostanie utworzona i dodane do naszych Eksploratora rozwiązań:</span><span class="sxs-lookup"><span data-stu-id="d3424-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="d3424-120">Tworzenie tabel w naszej bazie danych</span><span class="sxs-lookup"><span data-stu-id="d3424-120">Creating Tables within our Database</span></span>

<span data-ttu-id="d3424-121">Mamy teraz nowe pustej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d3424-121">We now have a new empty database.</span></span> <span data-ttu-id="d3424-122">Dodajmy niektóre tabele do niego.</span><span class="sxs-lookup"><span data-stu-id="d3424-122">Let's add some tables to it.</span></span>

<span data-ttu-id="d3424-123">W tym celu firma Microsoft będzie przejdź do okna kartę "Eksploratora serwera" w programie Visual Studio, która umożliwia firmie Microsoft w celu zarządzania bazami danych i serwerami.</span><span class="sxs-lookup"><span data-stu-id="d3424-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="d3424-124">Bazy danych programu SQL Server Express, przechowywane w \App\_folderu danych aplikacji automatycznie będzie wyświetlany w Eksploratorze serwera.</span><span class="sxs-lookup"><span data-stu-id="d3424-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="d3424-125">Firma Microsoft Opcjonalnie użyj ikony "Połączenia do bazy danych" w górnej części okna "Eksploratora serwera", aby dodać dodatkowe baz danych programu SQL Server (lokalne i zdalne) do listy również:</span><span class="sxs-lookup"><span data-stu-id="d3424-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="d3424-126">Dodamy dwóch tabel do NerdDinner bazy jest kompletny — jeden do przechowywania naszych kolacji, a drugi do śledzenia RSVP akceptacji.</span><span class="sxs-lookup"><span data-stu-id="d3424-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="d3424-127">Można utworzyć nowe tabele przez kliknięcie prawym przyciskiem myszy folder "Tabele" w naszej bazie danych i wybierając polecenie "Dodaj nową tabelę":</span><span class="sxs-lookup"><span data-stu-id="d3424-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="d3424-128">Spowoduje to otwarcie projektanta tabeli, który pozwala skonfigurować schemat naszych tabeli.</span><span class="sxs-lookup"><span data-stu-id="d3424-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="d3424-129">Dla tabeli naszych "Kolacji" dodamy 10 kolumny danych:</span><span class="sxs-lookup"><span data-stu-id="d3424-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="d3424-130">Chcemy kolumny "DinnerID", która ma być unikatowy klucz podstawowy dla tabeli.</span><span class="sxs-lookup"><span data-stu-id="d3424-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="d3424-131">Można to skonfigurować prawym przyciskiem myszy w kolumnie "DinnerID" i wybierając polecenie "Ustaw klucz podstawowy":</span><span class="sxs-lookup"><span data-stu-id="d3424-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="d3424-132">Oprócz tworzenia DinnerID klucza podstawowego, również chcemy, aby go skonfigurować jako kolumnę "identity", którego wartość jest automatycznie zwiększany w miarę dodawania nowych wierszy danych w tabeli (to znaczy pierwszy wiersz wstawiony obiad będzie mieć DinnerID 1, drugi wstawionego wiersza będą mieć DinnerID 2, itp).</span><span class="sxs-lookup"><span data-stu-id="d3424-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="d3424-133">Firma Microsoft to zrobić, wybierając kolumnę "DinnerID", a następnie za pomocą edytora "Kolumny właściwości" Ustaw właściwość "(jest tożsamość)" w kolumnie "Yes".</span><span class="sxs-lookup"><span data-stu-id="d3424-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="d3424-134">Będą używane wartości domyślne standardowe tożsamości (liczone od 1 i zwiększyć wartości 1 dla każdego nowego wiersza obiad):</span><span class="sxs-lookup"><span data-stu-id="d3424-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="d3424-135">Firma Microsoft będzie zapisaniu naszych tabelę, wpisując polecenie Ctrl-S lub za pomocą **pliku -&gt;zapisać** polecenia menu.</span><span class="sxs-lookup"><span data-stu-id="d3424-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="d3424-136">Wyświetla monit nam w nazwie tabeli.</span><span class="sxs-lookup"><span data-stu-id="d3424-136">This will prompt us to name the table.</span></span> <span data-ttu-id="d3424-137">Firma Microsoft będzie nadaj mu nazwę "Kolacji":</span><span class="sxs-lookup"><span data-stu-id="d3424-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="d3424-138">Naszej nowej tabeli kolacji następnie pojawi się w naszej bazie danych w Eksploratorze serwera.</span><span class="sxs-lookup"><span data-stu-id="d3424-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="d3424-139">Firma Microsoft będzie następnie powtórz powyższe kroki i Utwórz tabelę "RSVP".</span><span class="sxs-lookup"><span data-stu-id="d3424-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="d3424-140">Ta tabela z ma 3 kolumny.</span><span class="sxs-lookup"><span data-stu-id="d3424-140">This table with have 3 columns.</span></span> <span data-ttu-id="d3424-141">Firma Microsoft będzie konfiguracja kolumny RsvpID jako klucz podstawowy i wybierz kolumny tożsamości.</span><span class="sxs-lookup"><span data-stu-id="d3424-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="d3424-142">Firma Microsoft będzie zapisać go i nadaj mu nazwę "RSVP".</span><span class="sxs-lookup"><span data-stu-id="d3424-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="d3424-143">Definiowanie relacji klucza obcego między tabelami</span><span class="sxs-lookup"><span data-stu-id="d3424-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="d3424-144">Mamy teraz dwie tabele w naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d3424-144">We now have two tables within our database.</span></span> <span data-ttu-id="d3424-145">Naszych ostatni krok projektowania schematu będzie można skonfigurować relację "jeden do wielu" tych dwóch tabel — tak, aby każdy wiersz obiad można powiązane z zero lub więcej wierszy RSVP, które mają zastosowanie do niej.</span><span class="sxs-lookup"><span data-stu-id="d3424-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="d3424-146">Spowoduje to wykonanie przez skonfigurowanie tabeli RSVP "DinnerID" kolumnie relacji klucza obcego z kolumną "DinnerID" w tabeli "Kolacji".</span><span class="sxs-lookup"><span data-stu-id="d3424-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="d3424-147">W tym celu otworzymy tabeli RSVP w Projektancie tabel kliknij go dwukrotnie w Eksploratorze serwera.</span><span class="sxs-lookup"><span data-stu-id="d3424-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="d3424-148">Następnie wybierzemy kolumny "DinnerID", kliknij prawym przyciskiem myszy i wybierz polecenie "Relationshps..."</span><span class="sxs-lookup"><span data-stu-id="d3424-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…"</span></span> <span data-ttu-id="d3424-149">polecenia menu kontekstowe:</span><span class="sxs-lookup"><span data-stu-id="d3424-149">context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="d3424-150">Zostanie wyświetlone okno dialogowe, które możemy użyć do instalacji relacje między tabelami:</span><span class="sxs-lookup"><span data-stu-id="d3424-150">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="d3424-151">Firma Microsoft będzie kliknij przycisk "Dodaj", aby dodać nową relację do okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="d3424-151">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="d3424-152">Po dodaniu relacji firma Microsoft będzie rozwiń węzeł "Tabele i kolumny specyfikacji" widok drzewa w siatce właściwości z prawej strony okna dialogowego, a następnie kliknij przycisk "..." z prawej strony:</span><span class="sxs-lookup"><span data-stu-id="d3424-152">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="d3424-153">Klikając przycisk "...", zostanie wyświetlone okno dialogowe innego, która pozwala określić, które tabele i kolumny są uczestniczących w relacji, a także zezwolić nam na nazwę relacji.</span><span class="sxs-lookup"><span data-stu-id="d3424-153">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="d3424-154">Firma Microsoft będzie zmienić tabeli klucza podstawowego jako "Kolacji", a następnie wybierz kolumny "DinnerID" w tabeli kolacji jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="d3424-154">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="d3424-155">Nasze tabeli RSVP będzie tabeli klucza obcego i RSVP. Kolumna DinnerID zostaną skojarzone jako klucza obcego:</span><span class="sxs-lookup"><span data-stu-id="d3424-155">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="d3424-156">Teraz zostanie skojarzona z wiersza w tabeli obiad każdego wiersza w tabeli RSVP.</span><span class="sxs-lookup"><span data-stu-id="d3424-156">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="d3424-157">SQL Server będzie utrzymanie integralności referencyjnej nam — i uniemożliwiają nam dodanie nowego wiersza RSVP, jeśli go nie wskazuje prawidłowego wiersza obiad.</span><span class="sxs-lookup"><span data-stu-id="d3424-157">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="d3424-158">Ponadto uniemożliwi to nam usuwanie wiersza obiad, jeśli występują nadal RSVP wierszy odwołujące się do niego.</span><span class="sxs-lookup"><span data-stu-id="d3424-158">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="d3424-159">Dodawanie danych do naszej tabel</span><span class="sxs-lookup"><span data-stu-id="d3424-159">Adding Data to our Tables</span></span>

<span data-ttu-id="d3424-160">Teraz zakończyć dodawanie przykładowych danych do tabeli naszych kolacji.</span><span class="sxs-lookup"><span data-stu-id="d3424-160">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="d3424-161">Firma Microsoft dodać dane do tabeli, klikając go w Eksploratorze serwera i wybierając polecenie "Pokaż danych tabeli":</span><span class="sxs-lookup"><span data-stu-id="d3424-161">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="d3424-162">Dodamy kilka wierszy obiad możemy użyć później Rozpoczniemy wdrażania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="d3424-162">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="d3424-163">Następny krok</span><span class="sxs-lookup"><span data-stu-id="d3424-163">Next Step</span></span>

<span data-ttu-id="d3424-164">Po zakończeniu tworzenia naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="d3424-164">We've finished creating our database.</span></span> <span data-ttu-id="d3424-165">Teraz Utwórzmy klasy modeli, które możemy użyć do wykonywania zapytań i zaktualizować go.</span><span class="sxs-lookup"><span data-stu-id="d3424-165">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d3424-166">[Poprzednie](create-a-new-aspnet-mvc-project.md)
> [dalej](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="d3424-166">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
