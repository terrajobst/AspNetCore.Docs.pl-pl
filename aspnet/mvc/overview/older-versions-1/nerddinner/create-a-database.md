---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Tworzenie bazy danych | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 2 przedstawiono kroki, aby utworzyć bazę danych, zawierający wszystkie dinner i Odpowiedz na zaproszenie dane dla naszej aplikacji NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b6ab0740f251889f0fa0561809cac2bbe79bcb3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755169"
---
<a name="create-a-database"></a><span data-ttu-id="03992-103">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="03992-103">Create a Database</span></span>
====================
<span data-ttu-id="03992-104">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="03992-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="03992-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="03992-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="03992-106">Jest to krok 2 z BEZPŁATNEJ [samouczek aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , przeszukiwania — szczegółowe instrukcje dotyczące tworzenia małych, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="03992-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="03992-107">Krok 2 przedstawiono kroki, aby utworzyć bazę danych, zawierający wszystkie dinner i Odpowiedz na zaproszenie dane dla naszej aplikacji NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="03992-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="03992-108">Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.</span><span class="sxs-lookup"><span data-stu-id="03992-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="03992-109">NerdDinner krok 2: Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="03992-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="03992-110">Będziemy używać bazy danych do przechowywania wszystkich danych obiad i RSVP dla naszej aplikacji NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="03992-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="03992-111">W poniższych krokach przedstawiono tworzenie bazy danych przy użyciu bezpłatnego programu SQL Server Express edition (który można łatwo zainstalować, za pomocą programu w wersji 2 [Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="03992-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="03992-112">Cały kod, będziemy pisać w programach SQL Server Express i pełnego programu SQL Server.</span><span class="sxs-lookup"><span data-stu-id="03992-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="03992-113">Tworzenie nowej bazy danych programu SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="03992-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="03992-114">Firma Microsoft będzie zacząć, klikając prawym przyciskiem myszy w projekcie sieci web, a następnie wybierz **Add -&gt;nowy element** polecenia menu:</span><span class="sxs-lookup"><span data-stu-id="03992-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="03992-115">Zostanie wyświetlone okno dialogowe "Dodaj nowy element" Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="03992-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="03992-116">Firma Microsoft będzie filtrować według kategorii "Dane" i wybierz szablon elementu "Bazy danych SQL Server":</span><span class="sxs-lookup"><span data-stu-id="03992-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="03992-117">Firma Microsoft będzie nazwa programu SQL Server Express bazy danych, którą chcemy, aby utworzyć "NerdDinner.mdf" i kliknij ok.</span><span class="sxs-lookup"><span data-stu-id="03992-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="03992-118">Program Visual Studio będzie następnie poproś nam Jeśli chcemy dodać ten plik do naszych \App\_katalog danych (które katalogu jest już instalacji za pomocą odczytu i zapisu listy ACL zabezpieczeń):</span><span class="sxs-lookup"><span data-stu-id="03992-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="03992-119">Firma Microsoft będzie kliknij przycisk "Tak", i naszej nowej bazy danych zostanie utworzona i dodany do naszych Eksploratora rozwiązań:</span><span class="sxs-lookup"><span data-stu-id="03992-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="03992-120">Tworzenie tabel w naszej bazie danych</span><span class="sxs-lookup"><span data-stu-id="03992-120">Creating Tables within our Database</span></span>

<span data-ttu-id="03992-121">W efekcie powstał nowej pustej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="03992-121">We now have a new empty database.</span></span> <span data-ttu-id="03992-122">Dodajmy kilka tabel do niego.</span><span class="sxs-lookup"><span data-stu-id="03992-122">Let's add some tables to it.</span></span>

<span data-ttu-id="03992-123">W tym celu firma Microsoft będzie przejdź do okna kartę "Eksploratora serwera" w programie Visual Studio, która pozwala na zarządzanie bazami danych i serwerami.</span><span class="sxs-lookup"><span data-stu-id="03992-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="03992-124">Bazy danych programu SQL Server Express, przechowywane w \App\_folderu danych naszej aplikacji zostanie automatycznie pokazana w Eksploratorze serwera.</span><span class="sxs-lookup"><span data-stu-id="03992-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="03992-125">Firma Microsoft Opcjonalnie użyj ikony "Połączenia do bazy danych" w górnej części okna "Eksploratora serwera", aby dodać dodatkowe bazy danych SQL Server (zarówno lokalnych, jak i zdalnych) do listy, a także:</span><span class="sxs-lookup"><span data-stu-id="03992-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="03992-126">Dwie tabele zostanie dodany do bazy NerdDinner — jeden do przechowywania naszych kolacji, a druga do śledzenia akceptacje RSVP.</span><span class="sxs-lookup"><span data-stu-id="03992-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="03992-127">Możemy tworzyć nowe tabele, klikając prawym przyciskiem myszy w folderze "Tabele" w naszej bazie danych i wybierając polecenie "Dodaj nową tabelę" w menu:</span><span class="sxs-lookup"><span data-stu-id="03992-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="03992-128">Spowoduje to otwarcie projektanta tabel, który pozwala skonfigurował schemat tabeli.</span><span class="sxs-lookup"><span data-stu-id="03992-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="03992-129">Naszym spisie "Kolacji" dodamy 10 kolumny danych:</span><span class="sxs-lookup"><span data-stu-id="03992-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="03992-130">Chcemy, aby kolumny "DinnerID", aby mieć unikatowy klucz podstawowy dla tabeli.</span><span class="sxs-lookup"><span data-stu-id="03992-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="03992-131">Firma Microsoft można skonfigurować, klikając prawym przyciskiem myszy, w kolumnie "DinnerID" i wybierając pozycję "Ustaw klucz podstawowy" element menu:</span><span class="sxs-lookup"><span data-stu-id="03992-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="03992-132">Oprócz DinnerID klucza podstawowego, chcemy również skonfigurować ją jako kolumnę "tożsamość", którego wartość jest automatycznie zwiększany w miarę dodawania nowych wierszy danych do tabeli (to znaczy pierwszy wiersz wstawionego obiad odniesie DinnerID 1, drugi wstawionego wiersza będzie miał DinnerID 2, itp).</span><span class="sxs-lookup"><span data-stu-id="03992-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="03992-133">Możemy to zrobić, wybierając kolumnę "DinnerID", a następnie za pomocą edytora "Kolumny właściwości" można ustawić właściwości "(jest tożsamość)" dla kolumny "Yes".</span><span class="sxs-lookup"><span data-stu-id="03992-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="03992-134">Będziemy używać tożsamości standardowe wartości domyślne (liczone od 1 i zwiększ 1 dla każdego nowego wiersza obiad):</span><span class="sxs-lookup"><span data-stu-id="03992-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="03992-135">Następnie zostaną zapisane tabeli, wpisując Ctrl + S lub za pomocą **plikach&gt;Zapisz** polecenia menu.</span><span class="sxs-lookup"><span data-stu-id="03992-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="03992-136">Wyświetli monit nam nazwy tabeli.</span><span class="sxs-lookup"><span data-stu-id="03992-136">This will prompt us to name the table.</span></span> <span data-ttu-id="03992-137">Firma Microsoft będzie nadaj mu nazwę "Kolacji":</span><span class="sxs-lookup"><span data-stu-id="03992-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="03992-138">Naszej nowej tabeli kolacji zostanie następnie wyświetlane w naszej bazie danych w Eksploratorze serwera.</span><span class="sxs-lookup"><span data-stu-id="03992-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="03992-139">Następnie utworzymy Powtórz powyższe kroki i Utwórz tabelę "RSVP".</span><span class="sxs-lookup"><span data-stu-id="03992-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="03992-140">Ta tabela mająca mieć 3 kolumny.</span><span class="sxs-lookup"><span data-stu-id="03992-140">This table with have 3 columns.</span></span> <span data-ttu-id="03992-141">Firma Microsoft będzie skonfigurować kolumny RsvpID jako klucz podstawowy i wybierz kolumnę tożsamości:</span><span class="sxs-lookup"><span data-stu-id="03992-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="03992-142">Firma Microsoft będzie Zapisz go i nadaj mu nazwę "RSVP".</span><span class="sxs-lookup"><span data-stu-id="03992-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="03992-143">Definiowanie relacji klucza obcego między tabelami</span><span class="sxs-lookup"><span data-stu-id="03992-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="03992-144">Teraz mamy dwie tabele w naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="03992-144">We now have two tables within our database.</span></span> <span data-ttu-id="03992-145">Nasze ostatnim krokiem projektowania schematu będzie można skonfigurować "jeden do wielu" relacji między tymi dwiema tabelami — tak, aby firma Microsoft można skojarzyć każdy wiersz obiad z zero lub więcej wierszy RSVP, które go dotyczą.</span><span class="sxs-lookup"><span data-stu-id="03992-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="03992-146">Firma Microsoft będzie to zrobić, konfigurując tabeli RSVP "DinnerID" w kolumnie relacji klucza obcego z kolumną "DinnerID" w tabeli "Kolacji".</span><span class="sxs-lookup"><span data-stu-id="03992-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="03992-147">W tym celu firma Microsoft będzie otwierają RSVP tabeli w Projektancie tabel, klikając dwukrotnie plik w Eksploratorze serwera.</span><span class="sxs-lookup"><span data-stu-id="03992-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="03992-148">Następnie należy wybrać kolumnę "DinnerID" znajdujący się w nim, kliknij prawym przyciskiem myszy, a wybierz polecenia menu kontekstowego "Relationshps...":</span><span class="sxs-lookup"><span data-stu-id="03992-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="03992-149">Pojawi się okno dialogowe, które możemy użyć do konfiguracji relacji między tabelami:</span><span class="sxs-lookup"><span data-stu-id="03992-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="03992-150">Firma Microsoft będzie kliknij przycisk "Dodaj", aby dodać nową relację do okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="03992-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="03992-151">Po dodaniu relacji firma Microsoft będzie rozwiń węzeł "Tabele i kolumny Specyfikacja" widok drzewa w siatce właściwości po prawej stronie okna dialogowego, a następnie kliknij przycisk "...", z prawej strony:</span><span class="sxs-lookup"><span data-stu-id="03992-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="03992-152">Klikając przycisk "...", zostanie wyświetlone okno innego okna dialogowego, który pozwala określić, które tabele i kolumny są uczestniczących w relacji, a także umożliwiają nam nazwę relacji.</span><span class="sxs-lookup"><span data-stu-id="03992-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="03992-153">Firma Microsoft będzie zmienić tabeli klucza podstawowego, jako "Kolacji", a następnie wybierz kolumnę "DinnerID" w tabeli kolacji jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="03992-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="03992-154">Tabeli RSVP będzie Tabela klucza obcego i RSVP. Kolumna DinnerID ma być skojarzona jako klucza obcego:</span><span class="sxs-lookup"><span data-stu-id="03992-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="03992-155">Teraz każdy wiersz w tabeli RSVP zostanie skojarzona z wiersza w tabeli obiad.</span><span class="sxs-lookup"><span data-stu-id="03992-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="03992-156">Program SQL Server spowoduje zachowania więzów integralności dla nas — i uniemożliwiają nam dodanie nowego wiersza RSVP, jeśli go nie wskazuje prawidłowego wiersza obiad.</span><span class="sxs-lookup"><span data-stu-id="03992-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="03992-157">On również uniemożliwi nam usunięcie wierszy obiad, jeśli są nadal RSVP wierszy, odwołując się do niego.</span><span class="sxs-lookup"><span data-stu-id="03992-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="03992-158">Dodawanie danych do naszych tabel</span><span class="sxs-lookup"><span data-stu-id="03992-158">Adding Data to our Tables</span></span>

<span data-ttu-id="03992-159">Teraz zakończone przez dodawanie przykładowych danych do tabeli kolacji.</span><span class="sxs-lookup"><span data-stu-id="03992-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="03992-160">Możemy dodać dane do tabeli, kliknij prawym przyciskiem myszy go w Eksploratorze serwera i wybierając polecenie "Pokaż dane tabeli":</span><span class="sxs-lookup"><span data-stu-id="03992-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="03992-161">Dodamy kilka wierszy danych obiad, które możemy użyć później Rozpoczniemy wdrażanie aplikacji:</span><span class="sxs-lookup"><span data-stu-id="03992-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="03992-162">Następny krok</span><span class="sxs-lookup"><span data-stu-id="03992-162">Next Step</span></span>

<span data-ttu-id="03992-163">Po zakończeniu tworzenia naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="03992-163">We've finished creating our database.</span></span> <span data-ttu-id="03992-164">Utwórzmy teraz klasy modeli, których możemy użyć do wykonywania zapytań i zaktualizuj go.</span><span class="sxs-lookup"><span data-stu-id="03992-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="03992-165">[Poprzednie](create-a-new-aspnet-mvc-project.md)
> [dalej](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="03992-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
