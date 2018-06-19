---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: Przeciągnij i upuść za pośrednictwem ReorderList (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Kontrola ReorderList w zestawie narzędzi kontroli AJAX zapewnia listy, które można zmienić kolejności przez użytkownika za pomocą operacji przeciągania i upuszczania. Bieżąca kolejność listy są...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 42464d10f119e0ba51d5eebf2a67e76e3e419bda
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873459"
---
<a name="drag-and-drop-via-reorderlist-c"></a><span data-ttu-id="0b27f-104">Przeciągnij i upuść za pośrednictwem ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="0b27f-104">Drag and Drop via ReorderList (C#)</span></span>
====================
<span data-ttu-id="0b27f-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0b27f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0b27f-106">[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0b27f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span></span>

> <span data-ttu-id="0b27f-107">Kontrola ReorderList w zestawie narzędzi kontroli AJAX zapewnia listy, które można zmienić kolejności przez użytkownika za pomocą operacji przeciągania i upuszczania.</span><span class="sxs-lookup"><span data-stu-id="0b27f-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="0b27f-108">Bieżąca kolejność listy jest utrwalony na serwerze.</span><span class="sxs-lookup"><span data-stu-id="0b27f-108">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="0b27f-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="0b27f-109">Overview</span></span>

<span data-ttu-id="0b27f-110">`ReorderList` Formantu w zestawie narzędzi programu AJAX formant zawiera listę można zmienić kolejności przez użytkownika za pomocą operacji przeciągania i upuszczania.</span><span class="sxs-lookup"><span data-stu-id="0b27f-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="0b27f-111">Bieżąca kolejność listy jest utrwalony na serwerze.</span><span class="sxs-lookup"><span data-stu-id="0b27f-111">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="0b27f-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="0b27f-112">Steps</span></span>

<span data-ttu-id="0b27f-113">`ReorderList` Sterowanie obsługuje powiązanie danych z bazy danych do listy.</span><span class="sxs-lookup"><span data-stu-id="0b27f-113">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="0b27f-114">Najlepsze obsługuje ona również zapisywanie zmian w kolejności element listy do magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="0b27f-114">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="0b27f-115">W przykładzie użyto magazynu danych programu Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="0b27f-115">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="0b27f-116">Baza danych jest opcjonalne (i wolne) część instalacji programu Visual Studio, w tym wersja express.</span><span class="sxs-lookup"><span data-stu-id="0b27f-116">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="0b27f-117">Jest również dostępny jako osobny plik do pobrania w obszarze [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="0b27f-117">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="0b27f-118">Dla tego przykładu przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest nazywany `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; to ustawienie domyślne.</span><span class="sxs-lookup"><span data-stu-id="0b27f-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="0b27f-119">Jeśli różni się konfigurację, należy dostosować informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0b27f-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="0b27f-120">Najprostszym sposobem konfigurowania bazy danych jest użycie programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="0b27f-120">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="0b27f-121">Nawiąż połączenie z serwerem, kliknij go dwukrotnie `Databases` i utworzyć nową bazę danych (kliknij prawym przyciskiem myszy i wybierz polecenie `New Database`) o nazwie `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="0b27f-121">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="0b27f-122">W tej bazie danych, Utwórz nową tabelę o nazwie `AJAX` następujące cztery kolumny:</span><span class="sxs-lookup"><span data-stu-id="0b27f-122">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="0b27f-123">`id` (podstawowego klucza, liczbę całkowitą z zakresu tożsamości, not NULL)</span><span class="sxs-lookup"><span data-stu-id="0b27f-123">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="0b27f-124">`char` (char(1), NULL)</span><span class="sxs-lookup"><span data-stu-id="0b27f-124">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="0b27f-125">`description` (varchar(50), NULL)</span><span class="sxs-lookup"><span data-stu-id="0b27f-125">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="0b27f-126">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="0b27f-126">`position` (int, NULL)</span></span>


<span data-ttu-id="0b27f-127">[![Układ tabeli AJAX](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0b27f-127">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="0b27f-128">Układ tabeli AJAX ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0b27f-128">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span></span>


<span data-ttu-id="0b27f-129">Następnie należy wypełnić tabeli kilka wartości.</span><span class="sxs-lookup"><span data-stu-id="0b27f-129">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="0b27f-130">Należy pamiętać, że `position` kolumny posiada kolejność sortowania elementów.</span><span class="sxs-lookup"><span data-stu-id="0b27f-130">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="0b27f-131">[![Początkowe dane w tabeli AJAX](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="0b27f-131">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span></span>

<span data-ttu-id="0b27f-132">Początkowe dane w tabeli AJAX ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="0b27f-132">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span></span>


<span data-ttu-id="0b27f-133">Następnym krokiem wymaga, aby wygenerować `SqlDataSource` formantu do komunikowania się z nową bazę danych i tabeli.</span><span class="sxs-lookup"><span data-stu-id="0b27f-133">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="0b27f-134">Źródło danych musi obsługiwać `SELECT` i `UPDATE` poleceń SQL.</span><span class="sxs-lookup"><span data-stu-id="0b27f-134">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="0b27f-135">Po zmianie kolejności elementów listy `ReorderList` formant automatycznie przesyła dwóch wartości w źródle danych `Update` polecenia: Nowa pozycja i identyfikator elementu.</span><span class="sxs-lookup"><span data-stu-id="0b27f-135">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="0b27f-136">W związku z tym potrzeb źródła danych `<UpdateParameters>` te dwie wartości w sekcji:</span><span class="sxs-lookup"><span data-stu-id="0b27f-136">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="0b27f-137">`ReorderList` Wymaga formantu można ustawić następujące atrybuty:</span><span class="sxs-lookup"><span data-stu-id="0b27f-137">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="0b27f-138">`AllowReorder`: Czy można przestawiać elementy listy</span><span class="sxs-lookup"><span data-stu-id="0b27f-138">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="0b27f-139">`DataSourceID`: Identyfikator źródła danych</span><span class="sxs-lookup"><span data-stu-id="0b27f-139">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="0b27f-140">`DataKeyField`: Nazwa kolumny klucza podstawowego w źródle danych</span><span class="sxs-lookup"><span data-stu-id="0b27f-140">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="0b27f-141">`SortOrderField`: Kolumnie źródła danych udostępniający porządek sortowania dla elementów listy</span><span class="sxs-lookup"><span data-stu-id="0b27f-141">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="0b27f-142">W `<DragHandleTemplate>` i `<ItemTemplate>` sekcje, układ listy można dostosować.</span><span class="sxs-lookup"><span data-stu-id="0b27f-142">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="0b27f-143">Ponadto możliwe jest wiązania z danymi przy użyciu `Eval()` metody, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="0b27f-143">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="0b27f-144">Informacje o stylu CSS następujące (zawartymi w `<DragHandleTemplate>` sekcji `ReorderList` kontroli) upewnia się, że wskaźnik myszy zmienia się odpowiednio po jest przemieszczane nad przeciągnij uchwyt:</span><span class="sxs-lookup"><span data-stu-id="0b27f-144">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

<span data-ttu-id="0b27f-145">Na koniec `ScriptManager` formant inicjuje ASP.NET AJAX dla strony:</span><span class="sxs-lookup"><span data-stu-id="0b27f-145">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="0b27f-146">Uruchom w tym przykładzie w przeglądarce i nieco rozmieszczanie elementów listy.</span><span class="sxs-lookup"><span data-stu-id="0b27f-146">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="0b27f-147">Następnie ponownie załaduj stronę i/lub przyjrzeć bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0b27f-147">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="0b27f-148">Zmieniony pozycji została zachowana i odzwierciedlenie według wartości w `position` kolumny w bazie danych, a wszystko to bez żadnego kodu tylko za pomocą znacznika.</span><span class="sxs-lookup"><span data-stu-id="0b27f-148">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="0b27f-149">[![Kolejność elementów danych zgodnie z listą nowych zmian w bazie danych](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="0b27f-149">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span></span>

<span data-ttu-id="0b27f-150">Kolejność elementów danych zgodnie z listą nowych zmian w bazie danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="0b27f-150">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0b27f-151">[Poprzednie](using-postbacks-with-reorderlist-cs.md)
> [dalej](using-postbacks-with-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0b27f-151">[Previous](using-postbacks-with-reorderlist-cs.md)
[Next](using-postbacks-with-reorderlist-vb.md)</span></span>
