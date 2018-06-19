---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: Używanie ModalPopup z formantem elementu powtarzanego (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Istnieje również możliwość użycia tej zysk...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7124ca3fc346d78c3b235d1756695b008afb47aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872949"
---
<a name="using-modalpopup-with-a-repeater-control-c"></a><span data-ttu-id="043e3-104">Używanie ModalPopup z formantem elementu powtarzanego (C#)</span><span class="sxs-lookup"><span data-stu-id="043e3-104">Using ModalPopup with a Repeater Control (C#)</span></span>
====================
<span data-ttu-id="043e3-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="043e3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="043e3-106">[Pobierz kod](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="043e3-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)</span></span>

> <span data-ttu-id="043e3-107">Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="043e3-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="043e3-108">Istnieje również możliwość użycia tego formantu w obrębie elementu powtarzanego.</span><span class="sxs-lookup"><span data-stu-id="043e3-108">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="043e3-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="043e3-109">Overview</span></span>

<span data-ttu-id="043e3-110">Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="043e3-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="043e3-111">Istnieje również możliwość użycia tego formantu w obrębie elementu powtarzanego.</span><span class="sxs-lookup"><span data-stu-id="043e3-111">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="043e3-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="043e3-112">Steps</span></span>

<span data-ttu-id="043e3-113">Po pierwsze źródła danych jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="043e3-113">First of all, a data source is required.</span></span> <span data-ttu-id="043e3-114">W przykładzie użyto bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="043e3-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="043e3-115">Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobny plik do pobrania w obszarze [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="043e3-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="043e3-116">Baza danych AdventureWorks jest częścią programu SQL Server 2005 przykłady i przykładowe bazy danych (pobieranie pod [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="043e3-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="043e3-117">Najprostszym sposobem skonfigurowania bazy danych jest użycie programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie `AdventureWorks.mdf` pliku bazy danych.</span><span class="sxs-lookup"><span data-stu-id="043e3-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span> <span data-ttu-id="043e3-118">Dla tego przykładu przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest nazywany `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; to ustawienie domyślne.</span><span class="sxs-lookup"><span data-stu-id="043e3-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="043e3-119">Jeśli różni się konfigurację, należy dostosować informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="043e3-119">If your setup differs, you have to adapt the connection information for the database.</span></span> <span data-ttu-id="043e3-120">W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="043e3-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

<span data-ttu-id="043e3-121">Następnie Dodaj źródło danych do strony.</span><span class="sxs-lookup"><span data-stu-id="043e3-121">Then, add a data source to the page.</span></span> <span data-ttu-id="043e3-122">Aby skorzystać z ograniczoną ilość danych, możemy wybrać tylko pierwsze pięć wpisów w tabeli dostawcy bazy danych AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="043e3-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="043e3-123">Jeśli używasz programu Visual Studio Asystenta można utworzyć źródła danych, należy pamiętać, usterki w bieżącej wersji prefiksu nazwy tabeli (`Vendor`) z `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="043e3-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="043e3-124">Następujący kod przedstawia prawidłowa składnia:</span><span class="sxs-lookup"><span data-stu-id="043e3-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

<span data-ttu-id="043e3-125">Następnie dodaj panel, która służy jako modalnych menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="043e3-125">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="043e3-126">Zawiera on `Button` formantu, aby zamknąć menu podręcznego ponownie:</span><span class="sxs-lookup"><span data-stu-id="043e3-126">It contains a `Button` control to close the popup again:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

<span data-ttu-id="043e3-127">Aby podręcznego pracować w elemencie powtarzanym `ModalPopupExtender` formant musi znajdować się w obrębie `<ItemTemplate>` sekcji elementu powtarzanego.</span><span class="sxs-lookup"><span data-stu-id="043e3-127">In order to make the popup work within the repeater, the `ModalPopupExtender` control must be put within the `<ItemTemplate>` section of the repeater.</span></span> <span data-ttu-id="043e3-128">Dlatego panelu znajduje się poza powtarzanego, ale rozszerzeń znajduje się wewnątrz.</span><span class="sxs-lookup"><span data-stu-id="043e3-128">So the panel is outside the repeater, but the extender is inside.</span></span> <span data-ttu-id="043e3-129">Oto kod znaczników dla powtarzanego:</span><span class="sxs-lookup"><span data-stu-id="043e3-129">Here is the markup for the repeater:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

<span data-ttu-id="043e3-130">Następnie każdy element w źródle danych są wyświetlane obok niej przycisk wyzwalaną modalnych menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="043e3-130">Then, every item in the data source is displayed with a button next to it that triggers the modal popup.</span></span>


<span data-ttu-id="043e3-131">[![Modalne podręcznego można wywoływać dla każdego wpisu źródła danych](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="043e3-131">[![The modal popup can be triggered for every data source entry](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="043e3-132">Modalne podręcznego można wywoływać dla każdego wpisu źródła danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="043e3-132">The modal popup can be triggered for every data source entry ([Click to view full-size image](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="043e3-133">[Poprzednie](launching-a-modal-popup-window-from-server-code-cs.md)
> [dalej](handling-postbacks-from-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="043e3-133">[Previous](launching-a-modal-popup-window-from-server-code-cs.md)
[Next](handling-postbacks-from-a-modalpopup-cs.md)</span></span>
