---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
title: "Przy użyciu ConfirmButton w powtarzanym (C#) | Dokumentacja firmy Microsoft"
author: wenz
description: "Rozszerzenie ConfirmButton w zestawie narzędzi kontroli AJAX tworzy tak/nie podręcznego, gdy użytkownik kliknie przycisk (w tym LinkButton kontroli). Jest tak tylko wtedy, gdy..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a973ed3e-400c-4925-ace2-0b086b479301
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 12fd3e4e58e6a73720ade38372caff496f7f2c97
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-a-confirmbutton-in-a-repeater-c"></a><span data-ttu-id="682a2-104">Przy użyciu ConfirmButton w powtarzanym (C#)</span><span class="sxs-lookup"><span data-stu-id="682a2-104">Using a ConfirmButton In a Repeater (C#)</span></span>
====================
<span data-ttu-id="682a2-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="682a2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="682a2-106">[Pobierz kod](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="682a2-106">[Download Code](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)</span></span>

> <span data-ttu-id="682a2-107">Rozszerzenie ConfirmButton w zestawie narzędzi kontroli AJAX tworzy tak/nie podręcznego, gdy użytkownik kliknie przycisk (w tym LinkButton kontroli).</span><span class="sxs-lookup"><span data-stu-id="682a2-107">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="682a2-108">Tylko tak po kliknięciu przycisku akcji jest wykonywane, w przeciwnym razie anulowane.</span><span class="sxs-lookup"><span data-stu-id="682a2-108">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="682a2-109">To jest również w elementu powtarzanego.</span><span class="sxs-lookup"><span data-stu-id="682a2-109">This is also possible in a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="682a2-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="682a2-110">Overview</span></span>

<span data-ttu-id="682a2-111">Rozszerzenie ConfirmButton w zestawie narzędzi kontroli AJAX tworzy tak/nie podręcznego, gdy użytkownik kliknie przycisk (w tym LinkButton kontroli).</span><span class="sxs-lookup"><span data-stu-id="682a2-111">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="682a2-112">Tylko tak po kliknięciu przycisku akcji jest wykonywane, w przeciwnym razie anulowane.</span><span class="sxs-lookup"><span data-stu-id="682a2-112">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="682a2-113">To jest również w elementu powtarzanego.</span><span class="sxs-lookup"><span data-stu-id="682a2-113">This is also possible in a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="682a2-114">Kroki</span><span class="sxs-lookup"><span data-stu-id="682a2-114">Steps</span></span>

<span data-ttu-id="682a2-115">Po pierwsze źródła danych jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="682a2-115">First of all, a data source is required.</span></span> <span data-ttu-id="682a2-116">W przykładzie użyto bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="682a2-116">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="682a2-117">Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobny plik do pobrania w obszarze [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="682a2-117">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="682a2-118">Baza danych AdventureWorks jest częścią programu SQL Server 2005 przykłady i przykładowe bazy danych (pobieranie pod [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="682a2-118">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="682a2-119">Najprostszym sposobem skonfigurowania bazy danych jest użycie programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie `AdventureWorks.mdf` pliku bazy danych.</span><span class="sxs-lookup"><span data-stu-id="682a2-119">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="682a2-120">Dla tego przykładu przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest nazywany `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; to ustawienie domyślne.</span><span class="sxs-lookup"><span data-stu-id="682a2-120">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="682a2-121">Jeśli różni się konfigurację, należy dostosować informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="682a2-121">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="682a2-122">W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="682a2-122">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample1.aspx)]

<span data-ttu-id="682a2-123">Następnie źródła danych jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="682a2-123">Then, a data source is required.</span></span> <span data-ttu-id="682a2-124">Dla uproszczenia są pobierane tylko pierwsze pięć wpisów w tabeli dostawców AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="682a2-124">For the sake of simplicity, only the first five entries in AdventureWorks' Vendors table are retrieved.</span></span> <span data-ttu-id="682a2-125">Należy pamiętać, że podczas korzystania z Kreatora programu Visual Studio, aby utworzyć źródło danych, a nazwy tabeli (`Vendors`) aktualnie nie jest poprawnie poprzedzona z `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="682a2-125">Note that when using the Visual Studio wizard to create the data source, the table name (`Vendors`) is currently not correctly prefixed with `Purchasing`.</span></span> <span data-ttu-id="682a2-126">Następujący kod jest poprawny:</span><span class="sxs-lookup"><span data-stu-id="682a2-126">The following markup is the correct one:</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample2.aspx)]

<span data-ttu-id="682a2-127">Następnie można tego źródła danych w ramach elementu powtarzanego.</span><span class="sxs-lookup"><span data-stu-id="682a2-127">This data source can then be used within a repeater.</span></span> <span data-ttu-id="682a2-128">Jak zwykle `DataBinder.Eval()` metoda pobiera dane ze źródła danych.</span><span class="sxs-lookup"><span data-stu-id="682a2-128">As usual, the `DataBinder.Eval()` method retrieves data from the data source.</span></span> <span data-ttu-id="682a2-129">`ConfirmButtonExtender` Kontroli następnie musi się znajdować w `<ItemTemplate>` sekcji powtarzanego, że jest wyświetlany dla każdego wpisu w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="682a2-129">The `ConfirmButtonExtender` control must then be placed within the `<ItemTemplate>` section of the repeater so that it appears for every entry in the data source.</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample3.aspx)]


<span data-ttu-id="682a2-130">[![Przycisk Potwierdź widoczna obok każdego wpisu ze źródła danych](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="682a2-130">[![The confirm button appears next to each entry from the data source](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)</span></span>

<span data-ttu-id="682a2-131">Przycisk Potwierdź widoczna obok każdego wpisu ze źródła danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="682a2-131">The confirm button appears next to each entry from the data source ([Click to view full-size image](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="682a2-132">Dalej</span><span class="sxs-lookup"><span data-stu-id="682a2-132">Next</span></span>](using-a-confirmbutton-in-a-repeater-vb.md)
