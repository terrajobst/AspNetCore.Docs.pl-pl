---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
title: Przy użyciu ModalPopup o kontrolce elementu powtarzanego (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta. Istnieje również możliwość użycia tej zysk...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0c8e74f1-b3ba-4ca9-a1c5-f5c4831a359a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 04e3b132d1de2f42ba5de113dfbc22c85c3b198e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="using-modalpopup-with-a-repeater-control-vb"></a><span data-ttu-id="83e40-104">Przy użyciu ModalPopup o kontrolce elementu powtarzanego (VB)</span><span class="sxs-lookup"><span data-stu-id="83e40-104">Using ModalPopup with a Repeater Control (VB)</span></span>
====================
<span data-ttu-id="83e40-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="83e40-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="83e40-106">[Pobierz kod](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="83e40-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)</span></span>

> <span data-ttu-id="83e40-107">Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="83e40-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="83e40-108">Istnieje również możliwość użycia tego formantu w obrębie elementu powtarzanego.</span><span class="sxs-lookup"><span data-stu-id="83e40-108">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="83e40-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="83e40-109">Overview</span></span>

<span data-ttu-id="83e40-110">Formant ModalPopup w zestawie narzędzi kontroli AJAX pozwala w prosty sposób utworzyć modalnego okna podręcznego przy użyciu środków po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="83e40-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="83e40-111">Istnieje również możliwość użycia tego formantu w obrębie elementu powtarzanego.</span><span class="sxs-lookup"><span data-stu-id="83e40-111">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="83e40-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="83e40-112">Steps</span></span>

<span data-ttu-id="83e40-113">Po pierwsze źródła danych jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="83e40-113">First of all, a data source is required.</span></span> <span data-ttu-id="83e40-114">W przykładzie użyto bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="83e40-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="83e40-115">Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobny plik do pobrania w obszarze [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="83e40-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="83e40-116">Baza danych AdventureWorks jest częścią programu SQL Server 2005 przykłady i przykładowe bazy danych (pobieranie pod [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="83e40-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="83e40-117">Najprostszym sposobem skonfigurowania bazy danych jest użycie programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie `AdventureWorks.mdf` pliku bazy danych.</span><span class="sxs-lookup"><span data-stu-id="83e40-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span> <span data-ttu-id="83e40-118">Dla tego przykładu przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest nazywany `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; to ustawienie domyślne.</span><span class="sxs-lookup"><span data-stu-id="83e40-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="83e40-119">Jeśli różni się konfigurację, należy dostosować informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="83e40-119">If your setup differs, you have to adapt the connection information for the database.</span></span> <span data-ttu-id="83e40-120">W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="83e40-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="83e40-121">Następnie Dodaj źródło danych do strony.</span><span class="sxs-lookup"><span data-stu-id="83e40-121">Then, add a data source to the page.</span></span> <span data-ttu-id="83e40-122">Aby skorzystać z ograniczoną ilość danych, możemy wybrać tylko pierwsze pięć wpisów w tabeli dostawcy bazy danych AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="83e40-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="83e40-123">Jeśli używasz programu Visual Studio Asystenta można utworzyć źródła danych, należy pamiętać, usterki w bieżącej wersji prefiksu nazwy tabeli (`Vendor`) z `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="83e40-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="83e40-124">Następujący kod przedstawia prawidłowa składnia:</span><span class="sxs-lookup"><span data-stu-id="83e40-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="83e40-125">Następnie dodaj panel, która służy jako modalnych menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="83e40-125">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="83e40-126">Zawiera on `Button` formantu, aby zamknąć menu podręcznego ponownie:</span><span class="sxs-lookup"><span data-stu-id="83e40-126">It contains a `Button` control to close the popup again:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="83e40-127">Aby podręcznego pracować w elemencie powtarzanym `ModalPopupExtender` formant musi znajdować się w obrębie `<ItemTemplate>` sekcji elementu powtarzanego.</span><span class="sxs-lookup"><span data-stu-id="83e40-127">In order to make the popup work within the repeater, the `ModalPopupExtender` control must be put within the `<ItemTemplate>` section of the repeater.</span></span> <span data-ttu-id="83e40-128">Dlatego panelu znajduje się poza powtarzanego, ale rozszerzeń znajduje się wewnątrz.</span><span class="sxs-lookup"><span data-stu-id="83e40-128">So the panel is outside the repeater, but the extender is inside.</span></span> <span data-ttu-id="83e40-129">Oto kod znaczników dla powtarzanego:</span><span class="sxs-lookup"><span data-stu-id="83e40-129">Here is the markup for the repeater:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="83e40-130">Następnie każdy element w źródle danych są wyświetlane obok niej przycisk wyzwalaną modalnych menu podręczne.</span><span class="sxs-lookup"><span data-stu-id="83e40-130">Then, every item in the data source is displayed with a button next to it that triggers the modal popup.</span></span>


<span data-ttu-id="83e40-131">[![Modalne podręcznego można wywoływać dla każdego wpisu źródła danych](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="83e40-131">[![The modal popup can be triggered for every data source entry](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="83e40-132">Modalne podręcznego można wywoływać dla każdego wpisu źródła danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="83e40-132">The modal popup can be triggered for every data source entry ([Click to view full-size image](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="83e40-133">[Poprzednie](launching-a-modal-popup-window-from-server-code-vb.md)
> [dalej](handling-postbacks-from-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="83e40-133">[Previous](launching-a-modal-popup-window-from-server-code-vb.md)
[Next](handling-postbacks-from-a-modalpopup-vb.md)</span></span>
