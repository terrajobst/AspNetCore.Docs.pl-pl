---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
title: Używanie HoverMenu z formantem elementu powtarzanego (C#) | Dokumentacja firmy Microsoft
author: wenz
description: 'Kontrola HoverMenu w zestawie narzędzi kontroli AJAX zapewnia efekt proste menu podręczne: gdy wskaźnik myszy jest przesuwany nad elementem, wyskakującego w specifi...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e7700e7b-edc3-4183-a713-70e507cc7490
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ff7a7ce3469a020df069c1339993d8893092d875
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="using-hovermenu-with-a-repeater-control-c"></a><span data-ttu-id="03c22-103">Używanie HoverMenu z formantem elementu powtarzanego (C#)</span><span class="sxs-lookup"><span data-stu-id="03c22-103">Using HoverMenu with a Repeater Control (C#)</span></span>
====================
<span data-ttu-id="03c22-104">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="03c22-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="03c22-105">[Pobierz kod](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="03c22-105">[Download Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)</span></span>

> <span data-ttu-id="03c22-106">Kontrola HoverMenu w zestawie narzędzi kontroli AJAX zapewnia efekt proste menu podręczne: gdy wskaźnik myszy jest przesuwany nad elementem, wyskakującego na określonej pozycji.</span><span class="sxs-lookup"><span data-stu-id="03c22-106">The HoverMenu control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="03c22-107">Istnieje również możliwość użycia tego formantu w obrębie elementu powtarzanego.</span><span class="sxs-lookup"><span data-stu-id="03c22-107">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="03c22-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="03c22-108">Overview</span></span>

<span data-ttu-id="03c22-109">`HoverMenu` Formantu w zestawie narzędzi kontroli AJAX zapewnia efekt proste menu podręczne: gdy wskaźnik myszy jest przesuwany nad elementem, wyskakującego na określonej pozycji.</span><span class="sxs-lookup"><span data-stu-id="03c22-109">The `HoverMenu` control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="03c22-110">Istnieje również możliwość użycia tego formantu w obrębie elementu powtarzanego.</span><span class="sxs-lookup"><span data-stu-id="03c22-110">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="03c22-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="03c22-111">Steps</span></span>

<span data-ttu-id="03c22-112">Po pierwsze źródła danych jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="03c22-112">First of all, a data source is required.</span></span> <span data-ttu-id="03c22-113">W przykładzie użyto bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="03c22-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="03c22-114">Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobny plik do pobrania w obszarze [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="03c22-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="03c22-115">Baza danych AdventureWorks jest częścią programu SQL Server 2005 przykłady i przykładowe bazy danych (pobieranie pod [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="03c22-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="03c22-116">Najprostszym sposobem skonfigurowania bazy danych jest użycie programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie `AdventureWorks.mdf` pliku bazy danych.</span><span class="sxs-lookup"><span data-stu-id="03c22-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="03c22-117">Dla tego przykładu przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest nazywany `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; to ustawienie domyślne.</span><span class="sxs-lookup"><span data-stu-id="03c22-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="03c22-118">Jeśli różni się konfigurację, należy dostosować informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="03c22-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="03c22-119">W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="03c22-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample1.aspx)]

<span data-ttu-id="03c22-120">Następnie Dodaj źródło danych do strony.</span><span class="sxs-lookup"><span data-stu-id="03c22-120">Then, add a data source to the page.</span></span> <span data-ttu-id="03c22-121">Aby skorzystać z ograniczoną ilość danych, możemy wybrać tylko pierwsze pięć wpisów w tabeli dostawcy bazy danych AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="03c22-121">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="03c22-122">Jeśli używasz programu Visual Studio Asystenta można utworzyć źródła danych, należy pamiętać, usterki w bieżącej wersji prefiksu nazwy tabeli (`Vendor`) z `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="03c22-122">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="03c22-123">Następujący kod przedstawia prawidłowa składnia:</span><span class="sxs-lookup"><span data-stu-id="03c22-123">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample2.aspx)]

<span data-ttu-id="03c22-124">Następnie dodaj panel, która służy jako modalnych menu podręczne:</span><span class="sxs-lookup"><span data-stu-id="03c22-124">Next, add a panel which serves as the modal popup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample3.aspx)]

<span data-ttu-id="03c22-125">Teraz `HoverMenuExtender` wejścia play.</span><span class="sxs-lookup"><span data-stu-id="03c22-125">Now, the `HoverMenuExtender` comes into play.</span></span> <span data-ttu-id="03c22-126">Tak, aby każdy element w źródle danych pobiera własną menu podręczne, rozszerzający musi znajdować się w elemencie powtarzanym `<ItemTemplate>` sekcji.</span><span class="sxs-lookup"><span data-stu-id="03c22-126">So that every element in the data source gets its own popup, the extender must be put within the repeater's `<ItemTemplate>` section.</span></span> <span data-ttu-id="03c22-127">Oto kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="03c22-127">Here is the markup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample4.aspx)]

<span data-ttu-id="03c22-128">Teraz każdy element w źródle danych wyświetla okna podręcznego w prawo (`PopupPosition` atrybut) z opóźnieniem 50 milisekund (`PopDelay` atrybutu).</span><span class="sxs-lookup"><span data-stu-id="03c22-128">Now every item in the data source displays a popup to the right (`PopupPosition` attribute) after a delay of 50 milliseconds (`PopDelay` attribute).</span></span>


<span data-ttu-id="03c22-129">[![Zostanie wyświetlone hover menu obok każdego elementu w elemencie powtarzanym](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="03c22-129">[![The hover menu appears next to each item in the repeater](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="03c22-130">Zostanie wyświetlone hover menu obok każdego elementu w elemencie powtarzanym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="03c22-130">The hover menu appears next to each item in the repeater ([Click to view full-size image](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="03c22-131">Next</span><span class="sxs-lookup"><span data-stu-id="03c22-131">Next</span></span>](using-hovermenu-with-a-repeater-control-vb.md)
