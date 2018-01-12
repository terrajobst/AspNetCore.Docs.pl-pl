---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: "Wiązania z danymi do Accordion (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Accordion formantu w zestawie narzędzi kontroli AJAX zapewnia wiele okienek i umożliwia użytkownikowi wyświetlanie jeden z nich w czasie. Panele są zwykle deklarowanych w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: a8250f58655b8fe8638d8e7a7b084ee9c33fe986
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="databinding-to-an-accordion-c"></a><span data-ttu-id="6dd32-104">Wiązania z danymi do Accordion (C#)</span><span class="sxs-lookup"><span data-stu-id="6dd32-104">Databinding to an Accordion (C#)</span></span>
====================
<span data-ttu-id="6dd32-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6dd32-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6dd32-106">[Pobierz kod](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="6dd32-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span></span>

> <span data-ttu-id="6dd32-107">Accordion formantu w zestawie narzędzi kontroli AJAX zapewnia wiele okienek i umożliwia użytkownikowi wyświetlanie jeden z nich w czasie.</span><span class="sxs-lookup"><span data-stu-id="6dd32-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="6dd32-108">Panele są zwykle deklarowanych w samej strony, ale powiązanie ze źródłem danych zapewnia większą elastyczność.</span><span class="sxs-lookup"><span data-stu-id="6dd32-108">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="6dd32-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="6dd32-109">Overview</span></span>

<span data-ttu-id="6dd32-110">Accordion formantu w zestawie narzędzi kontroli AJAX zapewnia wiele okienek i umożliwia użytkownikowi wyświetlanie jeden z nich w czasie.</span><span class="sxs-lookup"><span data-stu-id="6dd32-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="6dd32-111">Panele są zwykle deklarowanych w samej strony, ale powiązanie ze źródłem danych zapewnia większą elastyczność.</span><span class="sxs-lookup"><span data-stu-id="6dd32-111">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="6dd32-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="6dd32-112">Steps</span></span>

<span data-ttu-id="6dd32-113">Po pierwsze źródła danych jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="6dd32-113">First of all, a data source is required.</span></span> <span data-ttu-id="6dd32-114">W przykładzie użyto bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="6dd32-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="6dd32-115">Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobny plik do pobrania w obszarze [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="6dd32-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="6dd32-116">Baza danych AdventureWorks jest częścią programu SQL Server 2005 przykłady i przykładowe bazy danych (pobieranie pod [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="6dd32-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="6dd32-117">Najprostszym sposobem skonfigurowania bazy danych jest użycie programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie `AdventureWorks.mdf` pliku bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6dd32-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="6dd32-118">Dla tego przykładu przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest nazywany `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; to ustawienie domyślne.</span><span class="sxs-lookup"><span data-stu-id="6dd32-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="6dd32-119">Jeśli różni się konfigurację, należy dostosować informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6dd32-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="6dd32-120">W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="6dd32-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

<span data-ttu-id="6dd32-121">Następnie Dodaj źródło danych do strony.</span><span class="sxs-lookup"><span data-stu-id="6dd32-121">Then, add a data source to the page.</span></span> <span data-ttu-id="6dd32-122">Aby skorzystać z ograniczoną ilość danych, możemy wybrać tylko pierwsze pięć wpisów w tabeli dostawcy bazy danych AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="6dd32-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="6dd32-123">Jeśli używasz programu Visual Studio Asystenta można utworzyć źródła danych, należy pamiętać, usterki w bieżącej wersji prefiksu nazwy tabeli (`Vendor`) z `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="6dd32-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="6dd32-124">Następujący kod przedstawia prawidłowa składnia:</span><span class="sxs-lookup"><span data-stu-id="6dd32-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

<span data-ttu-id="6dd32-125">Zapamiętaj nazwę źródła danych (ID).</span><span class="sxs-lookup"><span data-stu-id="6dd32-125">Remember the name (ID) of the data source.</span></span> <span data-ttu-id="6dd32-126">Ten sam identyfikator musi być następnie używany w `DataSourceID` właściwości formantu Accordion:</span><span class="sxs-lookup"><span data-stu-id="6dd32-126">This very identification must then be used in the `DataSourceID` property of the Accordion control:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

<span data-ttu-id="6dd32-127">W formancie Accordion, możesz podać szablonów dla różnych części kontrolki, łącznie z nagłówkiem (`<HeaderTemplate>`) i zawartości (`<ContentTemplate>`).</span><span class="sxs-lookup"><span data-stu-id="6dd32-127">Within the Accordion control, you can provide templates for various parts of the control, including the header (`<HeaderTemplate>`) and the content (`<ContentTemplate>`).</span></span> <span data-ttu-id="6dd32-128">W ramach tych elementów, wyświetla dane z danych źródle, przy użyciu `DataBinder.Eval()` metody:</span><span class="sxs-lookup"><span data-stu-id="6dd32-128">Within these elements, just output the data from the data source, using the `DataBinder.Eval()` method:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

<span data-ttu-id="6dd32-129">Podczas ładowania strony źródła danych musi być powiązana accordion o tym kodzie po stronie serwera:</span><span class="sxs-lookup"><span data-stu-id="6dd32-129">When the page is loaded, the data source must be bound to the accordion with this server-side code:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

<span data-ttu-id="6dd32-130">Do podpisania tego przykładu, musisz zdefiniować dwa klas CSS, do których istnieją odwołania w formancie Accordion (w jego właściwościach `HeaderCssClass` i `ContentCssClass`).</span><span class="sxs-lookup"><span data-stu-id="6dd32-130">To conclude this sample, you need to define the two CSS classes that are referenced in the Accordion control (in its properties `HeaderCssClass` and `ContentCssClass`).</span></span> <span data-ttu-id="6dd32-131">Umieść następujący kod w `<head>` sekcji strony:</span><span class="sxs-lookup"><span data-stu-id="6dd32-131">Put the following markup in the `<head>` section of the page:</span></span>

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


<span data-ttu-id="6dd32-132">[![Dane w accordion pochodzi bezpośrednio ze źródła danych](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6dd32-132">[![The data in the accordion comes directly from the data source](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)</span></span>

<span data-ttu-id="6dd32-133">Dane w accordion pochodzi bezpośrednio ze źródła danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](databinding-to-an-accordion-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6dd32-133">The data in the accordion comes directly from the data source ([Click to view full-size image](databinding-to-an-accordion-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="6dd32-134">Dalej</span><span class="sxs-lookup"><span data-stu-id="6dd32-134">Next</span></span>](dynamically-adding-an-accordion-pane-cs.md)