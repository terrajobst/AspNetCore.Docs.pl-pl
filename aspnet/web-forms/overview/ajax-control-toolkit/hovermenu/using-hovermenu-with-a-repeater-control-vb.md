---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: Używanie kontrolki HoverMenu z kontrolką elementu powtarzanego (VB) | Dokumentacja firmy Microsoft
author: wenz
description: 'Na formant kontrolki HoverMenu z zestawu narzędzi AJAX Control Toolkit zawiera efekt proste okno podręczne: po zatrzymaniu wskaźnika myszy nad elementem, okno wyskakujące pojawia się na specifi...'
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: a00d2c610696672a36ebbb78b80abec6c5150101
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821346"
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a><span data-ttu-id="5b819-103">Używanie kontrolki HoverMenu z kontrolką elementu powtarzanego (VB)</span><span class="sxs-lookup"><span data-stu-id="5b819-103">Using HoverMenu with a Repeater Control (VB)</span></span>
====================
<span data-ttu-id="5b819-104">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5b819-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5b819-105">[Pobierz program Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5b819-105">[Download Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span></span>

> <span data-ttu-id="5b819-106">Na formant kontrolki HoverMenu z zestawu narzędzi AJAX Control Toolkit zawiera efekt proste okno podręczne: po zatrzymaniu wskaźnika myszy nad elementem, okno wyskakujące pojawia się na określonej pozycji.</span><span class="sxs-lookup"><span data-stu-id="5b819-106">The HoverMenu control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="5b819-107">Użytkownik może również użyć tej kontrolki w elemencie powtarzanym.</span><span class="sxs-lookup"><span data-stu-id="5b819-107">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="5b819-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="5b819-108">Overview</span></span>

<span data-ttu-id="5b819-109">`HoverMenu` Formantu w zestawu narzędzi AJAX Control Toolkit zawiera efekt proste okno podręczne: po zatrzymaniu wskaźnika myszy nad elementem, okno wyskakujące pojawia się na określonej pozycji.</span><span class="sxs-lookup"><span data-stu-id="5b819-109">The `HoverMenu` control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="5b819-110">Użytkownik może również użyć tej kontrolki w elemencie powtarzanym.</span><span class="sxs-lookup"><span data-stu-id="5b819-110">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="5b819-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="5b819-111">Steps</span></span>

<span data-ttu-id="5b819-112">Po pierwsze źródła danych jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="5b819-112">First of all, a data source is required.</span></span> <span data-ttu-id="5b819-113">Ta próbka używa bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="5b819-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="5b819-114">Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobnego pobrania w ramach [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="5b819-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="5b819-115">Bazy danych AdventureWorks jest częścią przykładów programu SQL Server 2005 i przykładowych baz danych (będą pobierane w [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="5b819-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="5b819-116">Najprostszym sposobem skonfigurowania bazy danych jest użycie, programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączyć `AdventureWorks.mdf` plik bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5b819-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="5b819-117">W tym przykładzie przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest wywoływana `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; jest domyślna konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="5b819-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="5b819-118">Jeśli różni się konfigurację, trzeba dostosować informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5b819-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="5b819-119">W celu włączenia funkcji ASP.NET AJAX i zestaw narzędzi do sterowania `ScriptManager` kontroli muszą znajdować się gdziekolwiek na stronie (ale poziomu `<form>` elementu):</span><span class="sxs-lookup"><span data-stu-id="5b819-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="5b819-120">Następnie dodaj źródła danych do strony.</span><span class="sxs-lookup"><span data-stu-id="5b819-120">Then, add a data source to the page.</span></span> <span data-ttu-id="5b819-121">Aby skorzystać z ograniczoną ilością danych, możemy wybrać tylko pięć pierwszych wpisów w tabeli Dostawca bazy danych AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="5b819-121">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="5b819-122">Jeśli są przy użyciu Asystenta ustawień programu Visual Studio, aby utworzyć źródło danych, należy uwzględnić następujące kwestie usterkę w bieżącej wersji prefiksu nazwy tabeli (`Vendor`) przy użyciu `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="5b819-122">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="5b819-123">Następujący kod przedstawia poprawnej składni:</span><span class="sxs-lookup"><span data-stu-id="5b819-123">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="5b819-124">Następnie dodaj panel, który służy jako modalne okno podręczne:</span><span class="sxs-lookup"><span data-stu-id="5b819-124">Next, add a panel which serves as the modal popup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="5b819-125">Teraz `HoverMenuExtender` trafia do odtwarzania.</span><span class="sxs-lookup"><span data-stu-id="5b819-125">Now, the `HoverMenuExtender` comes into play.</span></span> <span data-ttu-id="5b819-126">Tak, aby każdy element w źródle danych uzyskuje swój własny okna podręcznego, urządzenia extender muszą znajdować się w elemencie powtarzanym `<ItemTemplate>` sekcji.</span><span class="sxs-lookup"><span data-stu-id="5b819-126">So that every element in the data source gets its own popup, the extender must be put within the repeater's `<ItemTemplate>` section.</span></span> <span data-ttu-id="5b819-127">Oto znaczniki:</span><span class="sxs-lookup"><span data-stu-id="5b819-127">Here is the markup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="5b819-128">Teraz każdy element w źródle danych wyświetla wyskakującego okienka po prawej stronie (`PopupPosition` atrybutu) z opóźnieniem 50 MS (`PopDelay` atrybutu).</span><span class="sxs-lookup"><span data-stu-id="5b819-128">Now every item in the data source displays a popup to the right (`PopupPosition` attribute) after a delay of 50 milliseconds (`PopDelay` attribute).</span></span>


<span data-ttu-id="5b819-129">[![W menu po wskazaniu wskaźnikiem pojawia się obok każdego elementu w elemencie powtarzanym](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5b819-129">[![The hover menu appears next to each item in the repeater](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="5b819-130">W menu po wskazaniu wskaźnikiem pojawia się obok każdego elementu w elemencie powtarzanym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5b819-130">The hover menu appears next to each item in the repeater ([Click to view full-size image](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5b819-131">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="5b819-131">Previous</span></span>](using-hovermenu-with-a-repeater-control-cs.md)
