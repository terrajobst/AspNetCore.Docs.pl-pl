---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: "Przy użyciu CascadingDropDown z bazy danych (VB) | Dokumentacja firmy Microsoft"
author: wenz
description: "Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w anoth..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 65b9a499dd9b500338ccdb90e56b742ff50a1024
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-cascadingdropdown-with-a-database-vb"></a><span data-ttu-id="314fe-103">Przy użyciu CascadingDropDown z bazy danych (VB)</span><span class="sxs-lookup"><span data-stu-id="314fe-103">Using CascadingDropDown with a Database (VB)</span></span>
====================
<span data-ttu-id="314fe-104">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="314fe-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="314fe-105">[Pobierz kod](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="314fe-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)</span></span>

> <span data-ttu-id="314fe-106">Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w innym DropDownList.</span><span class="sxs-lookup"><span data-stu-id="314fe-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="314fe-107">Aby to zrobić można utworzyć usługi sieci web specjalnych.</span><span class="sxs-lookup"><span data-stu-id="314fe-107">In order for this to work, a special web service must be created.</span></span>


## <a name="overview"></a><span data-ttu-id="314fe-108">Omówienie</span><span class="sxs-lookup"><span data-stu-id="314fe-108">Overview</span></span>

<span data-ttu-id="314fe-109">Formant CascadingDropDown w zestawie narzędzi kontroli AJAX rozszerza formantu DropDownList tak, aby zmiany w jednej obciążeń DropDownList skojarzone wartości w innym DropDownList.</span><span class="sxs-lookup"><span data-stu-id="314fe-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="314fe-110">(Na przykład jedna lista zawiera listę nam stanów i dalej listy jest następnie wypełnione większych miastach w tym stanie.) Aby to zrobić można utworzyć usługi sieci web specjalnych.</span><span class="sxs-lookup"><span data-stu-id="314fe-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) In order for this to work, a special web service must be created.</span></span>

## <a name="steps"></a><span data-ttu-id="314fe-111">Kroki</span><span class="sxs-lookup"><span data-stu-id="314fe-111">Steps</span></span>

<span data-ttu-id="314fe-112">Po pierwsze źródła danych jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="314fe-112">First of all, a data source is required.</span></span> <span data-ttu-id="314fe-113">W przykładzie użyto bazy danych AdventureWorks i Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="314fe-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="314fe-114">Baza danych jest opcjonalnym składnikiem instalacji programu Visual Studio (w tym wersja express) i jest również dostępny jako osobny plik do pobrania w obszarze [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="314fe-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="314fe-115">Baza danych AdventureWorks jest częścią programu SQL Server 2005 przykłady i przykładowe bazy danych (pobieranie pod [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="314fe-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="314fe-116">Najprostszym sposobem skonfigurowania bazy danych jest użycie programu Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) i dołączenie `AdventureWorks.mdf` pliku bazy danych.</span><span class="sxs-lookup"><span data-stu-id="314fe-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="314fe-117">Dla tego przykładu przyjęto założenie, że wystąpienie programu SQL Server 2005 Express Edition jest nazywany `SQLEXPRESS` i znajduje się na tym samym komputerze co serwer sieci web; to ustawienie domyślne.</span><span class="sxs-lookup"><span data-stu-id="314fe-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="314fe-118">Jeśli różni się konfigurację, należy dostosować informacje o połączeniu dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="314fe-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="314fe-119">W celu aktywowania funkcji programu ASP.NET AJAX i Toolkit kontroli `ScriptManager` formant musi znajdować się dowolne miejsce na stronie (ale poziomu &lt; `form` &gt; element):</span><span class="sxs-lookup"><span data-stu-id="314fe-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

<span data-ttu-id="314fe-120">W następnym kroku wymagane są dwa formanty DropDownList.</span><span class="sxs-lookup"><span data-stu-id="314fe-120">In the next step, two DropDownList controls are required.</span></span> <span data-ttu-id="314fe-121">W tym przykładzie używamy dostawcy i skontaktuj się z informacji z AdventureWorks, dlatego utworzymy jedną listę dostępnych dostawców i jedną dla kontaktów, dostępne:</span><span class="sxs-lookup"><span data-stu-id="314fe-121">In this sample, we use the vendor and contact information from AdventureWorks, thus we create one list for the available vendors and one for the available contacts:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

<span data-ttu-id="314fe-122">Następnie dwóch rozszerzeń CascadingDropDown musi być dodane do strony.</span><span class="sxs-lookup"><span data-stu-id="314fe-122">Then, two CascadingDropDown extenders must be added to the page.</span></span> <span data-ttu-id="314fe-123">Wstawia jedną z pierwszej listy (dostawcy), a innego wypełnia druga lista (kontaktów).</span><span class="sxs-lookup"><span data-stu-id="314fe-123">One fills the first (vendors) list, and the other one fills the second (contacts) list.</span></span> <span data-ttu-id="314fe-124">Musi być ustawione następujące atrybuty:</span><span class="sxs-lookup"><span data-stu-id="314fe-124">The following attributes must be set:</span></span>

- <span data-ttu-id="314fe-125">`ServicePath`: Adres URL usługi sieci web dostarczania pozycji na liście</span><span class="sxs-lookup"><span data-stu-id="314fe-125">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="314fe-126">`ServiceMethod`: Dostarczanie pozycji na liście metody sieci web</span><span class="sxs-lookup"><span data-stu-id="314fe-126">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="314fe-127">`TargetControlID`: Identyfikator listy rozwijanej</span><span class="sxs-lookup"><span data-stu-id="314fe-127">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="314fe-128">`Category`: Kategoria informacje przesyłane podczas wywoływania metody sieci web</span><span class="sxs-lookup"><span data-stu-id="314fe-128">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="314fe-129">`PromptText`: Tekst wyświetlany, gdy asynchronicznie podczas ładowania danych listy z serwera</span><span class="sxs-lookup"><span data-stu-id="314fe-129">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>
- <span data-ttu-id="314fe-130">`ParentControlID`: (opcjonalnie) nadrzędnej listy rozwijanej liście ładowania tego wyzwalaczy bieżącej listy</span><span class="sxs-lookup"><span data-stu-id="314fe-130">`ParentControlID`: (optional) parent dropdown list that triggers loading of the current list</span></span>

<span data-ttu-id="314fe-131">W zależności od języka programowania, używany zmienia nazwę danej usługi sieci web, ale wszystkie wartości atrybutów są takie same.</span><span class="sxs-lookup"><span data-stu-id="314fe-131">Depending on the programming language used, the name of the web service in question changes, but all other attribute values are the same.</span></span> <span data-ttu-id="314fe-132">Oto elementu CascadingDropDown dla pierwszej listy rozwijanej:</span><span class="sxs-lookup"><span data-stu-id="314fe-132">Here is the CascadingDropDown element for the first dropdown list:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

<span data-ttu-id="314fe-133">Należy ustawić rozszerzeń formantu druga lista `ParentControlID` atrybutu, aby wybranie wpis w wyzwalaczy listy dostawców ładowania skojarzonych elementów z listy kontaktów.</span><span class="sxs-lookup"><span data-stu-id="314fe-133">The control extenders for the second list need to set the `ParentControlID` attribute so that selecting an entry in the vendors list triggers loading associated elements in the contacts list.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

<span data-ttu-id="314fe-134">Rzeczywista praca odbywa się następnie usługi sieci web, który został ustawiony w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="314fe-134">The actual work is then done in the web service, which is set up as follows.</span></span> <span data-ttu-id="314fe-135">Należy pamiętać, że `[ScriptService]` atrybut jest używany, w przeciwnym razie ASP.NET AJAX nie można utworzyć serwera proxy JavaScript dostęp do metody sieci web z poziomu kodu skryptu po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="314fe-135">Note that the `[ScriptService]` attribute is used, otherwise ASP.NET AJAX cannot create the JavaScript proxy to access the web methods from client-side script code.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

<span data-ttu-id="314fe-136">Podpis metody sieci web o nazwie przez CascadingDropDown wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="314fe-136">The signature of the web methods called by CascadingDropDown is as follows:</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

<span data-ttu-id="314fe-137">Dlatego zwracana wartość musi być tablicą typu `CascadingDropDownNameValue` którego jest definiowana za pomocą zestawu narzędzi kontroli.</span><span class="sxs-lookup"><span data-stu-id="314fe-137">So the return value must be an array of type `CascadingDropDownNameValue` which is defined by the Control Toolkit.</span></span> <span data-ttu-id="314fe-138">`GetVendors()` Metody jest bardzo łatwa do wdrożenia: kod nawiązuje połączenie z bazą danych AdventureWorks i wysyła zapytanie do dostawcy pierwsze 25.</span><span class="sxs-lookup"><span data-stu-id="314fe-138">The `GetVendors()` method is quite easy to implement: The code connects to the AdventureWorks database and queries the first 25 vendors.</span></span> <span data-ttu-id="314fe-139">Pierwszy parametr w `CascadingDropDownNameValue` Konstruktor jest jego wartość podpis wpis listy, a drugi (wartość atrybutu w formacie HTML w &lt; `option` &gt; elementu).</span><span class="sxs-lookup"><span data-stu-id="314fe-139">The first parameter in the `CascadingDropDownNameValue` constructor is the caption of the list entry, the second one its value (value attribute in HTML's &lt;`option`&gt; element).</span></span> <span data-ttu-id="314fe-140">Oto kod:</span><span class="sxs-lookup"><span data-stu-id="314fe-140">Here is the code:</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

<span data-ttu-id="314fe-141">Pobieranie skojarzone kontakty dla dostawcy (Nazwa metody: `GetContactsForVendor()`) jest nieco trudniejszy.</span><span class="sxs-lookup"><span data-stu-id="314fe-141">Getting the associated contacts for a vendor (method name: `GetContactsForVendor()`) is a bit trickier.</span></span> <span data-ttu-id="314fe-142">Po pierwsze należy określić dostawcę, który został wybrany z pierwszej listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="314fe-142">First of all, the vendor which has been selected in the first dropdown list must be determined.</span></span> <span data-ttu-id="314fe-143">Zestaw narzędzi do sterowania definiuje metodę pomocniczą dla tego zadania: `ParseKnownCategoryValuesString()` metoda zwraca `StringDictionary` element z listy rozwijanej danych:</span><span class="sxs-lookup"><span data-stu-id="314fe-143">The Control Toolkit defines a helper method for that task: The `ParseKnownCategoryValuesString()` method returns a `StringDictionary` element with the dropdown data:</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

<span data-ttu-id="314fe-144">Ze względów bezpieczeństwa danych musi najpierw zostać zweryfikowany.</span><span class="sxs-lookup"><span data-stu-id="314fe-144">For security reasons, this data must be validated first.</span></span> <span data-ttu-id="314fe-145">Tak, jeśli istnieje wpis dostawcy (ponieważ `Category` ma ustawioną właściwość pierwszego elementu CascadingDropDown `"Vendor"`), można pobrać identyfikator wybranego dostawcy:</span><span class="sxs-lookup"><span data-stu-id="314fe-145">So if there is a Vendor entry (because the `Category` property of the first CascadingDropDown element is set to `"Vendor"`), the ID of the selected vendor may be retrieved:</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

<span data-ttu-id="314fe-146">Pozostałe metody jest dość proste, następnie.</span><span class="sxs-lookup"><span data-stu-id="314fe-146">The rest of the method is fairly straight-forward, then.</span></span> <span data-ttu-id="314fe-147">Identyfikator dostawcy jest używany jako parametr dla zapytania SQL, które pobiera wszystkie kontakty skojarzone dla tego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="314fe-147">The vendor's ID is used as a parameter for an SQL query that retrieves all associated contacts for that vendor.</span></span> <span data-ttu-id="314fe-148">Ponownie, metoda zwraca tablicę typu `CascadingDropDownNameValue`.</span><span class="sxs-lookup"><span data-stu-id="314fe-148">Once again, the method returns an array of type `CascadingDropDownNameValue`.</span></span>

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

<span data-ttu-id="314fe-149">Ładowanie strony ASP.NET, a po chwili krótkich listy dostawców jest wypełniony 25 wpisów.</span><span class="sxs-lookup"><span data-stu-id="314fe-149">Load the ASP.NET page, and after a short while the vendor list is filled with 25 entries.</span></span> <span data-ttu-id="314fe-150">Wybierz jedną pozycję i zwróć uwagę, jak drugi listy rozwijanej jest wypełniony danych.</span><span class="sxs-lookup"><span data-stu-id="314fe-150">Pick one entry and notice how the second dropdown list is filled with data.</span></span>


<span data-ttu-id="314fe-151">[![Pierwsza lista jest wypełniane automatycznie](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="314fe-151">[![The first list is filled automatically](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)</span></span>

<span data-ttu-id="314fe-152">Pierwsza lista jest wypełniana automatycznie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-cascadingdropdown-with-a-database-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="314fe-152">The first list is filled automatically ([Click to view full-size image](using-cascadingdropdown-with-a-database-vb/_static/image3.png))</span></span>


<span data-ttu-id="314fe-153">[![Druga lista jest wypełniana zgodnie z wyborem z pierwszej listy](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="314fe-153">[![The second list is filled according to the selection in the first list](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)</span></span>

<span data-ttu-id="314fe-154">Druga lista jest wypełniana zgodnie z wyborem z pierwszej listy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-cascadingdropdown-with-a-database-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="314fe-154">The second list is filled according to the selection in the first list ([Click to view full-size image](using-cascadingdropdown-with-a-database-vb/_static/image6.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="314fe-155">[Poprzednie](filling-a-list-using-cascadingdropdown-vb.md)
[dalej](presetting-list-entries-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="314fe-155">[Previous](filling-a-list-using-cascadingdropdown-vb.md)
[Next](presetting-list-entries-with-cascadingdropdown-vb.md)</span></span>
