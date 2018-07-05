---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Część 6: Członkostwo ASP.NET | Dokumentacja firmy Microsoft'
author: JoeStagner
description: W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 6 dodaje członkostwa ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6d01a745ca1428f065f564f676d483ee807eb52e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368476"
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="bfda6-104">Część 6: Członkostwo ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bfda6-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="bfda6-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="bfda6-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="bfda6-106">Tailspin Spyworks pokazuje, jak bardzo łatwo jest tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="bfda6-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="bfda6-107">Przedstawia on poza sposób użycia wspaniałych nowych funkcjach w ASP.NET 4 do tworzenia sklep online, m.in. zakupy wyewidencjonowanie i Administracja.</span><span class="sxs-lookup"><span data-stu-id="bfda6-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="bfda6-108">W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="bfda6-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="bfda6-109">Część 6 dodaje członkostwa ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bfda6-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a>  <span data-ttu-id="bfda6-110">Praca z członkostwa ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bfda6-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="bfda6-111">Kliknij pozycję Zabezpieczenia</span><span class="sxs-lookup"><span data-stu-id="bfda6-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="bfda6-112">Upewnij się, że używasz uwierzytelniania formularzy.</span><span class="sxs-lookup"><span data-stu-id="bfda6-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="bfda6-113">Użyj linku "Utwórz użytkownika", aby utworzyć kilka użytkowników.</span><span class="sxs-lookup"><span data-stu-id="bfda6-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="bfda6-114">Gdy skończysz, można znaleźć w oknie Eksploratora rozwiązań, a następnie Odśwież widok.</span><span class="sxs-lookup"><span data-stu-id="bfda6-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="bfda6-115">Należy pamiętać, że ASPNETDB. Utworzono MDF dobrym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="bfda6-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="bfda6-116">Ten plik zawiera tabele do obsługi usług ASP.NET core, takich jak członkostwo.</span><span class="sxs-lookup"><span data-stu-id="bfda6-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="bfda6-117">Można teraz rozpocząć implementowanie rozpoczęcie procesu realizowania zamówienia.</span><span class="sxs-lookup"><span data-stu-id="bfda6-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="bfda6-118">Rozpocznij od utworzenia strony CheckOut.aspx.</span><span class="sxs-lookup"><span data-stu-id="bfda6-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="bfda6-119">Na stronie CheckOut.aspx powinny być dostępne jedynie dla użytkowników, którzy są zalogowani, dzięki czemu będziemy ograniczać przydziału obowiązków dostęp do rejestrowane w przystawce Użytkownicy i przekierowuje użytkowników, którzy nie jest zalogowany do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="bfda6-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="bfda6-120">W tym celu dodamy następujących sekcji konfiguracji naszego pliku web.config.</span><span class="sxs-lookup"><span data-stu-id="bfda6-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="bfda6-121">Szablon dla aplikacji formularzy sieci Web programu ASP.NET dodano sekcję uwierzytelniania do naszego pliku web.config i automatycznie nawiązane domyślną stronę logowania.</span><span class="sxs-lookup"><span data-stu-id="bfda6-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="bfda6-122">Firma Microsoft należy zmodyfikować kod Login.aspx związany z pliku do migracji anonimowe koszyka, jeśli użytkownik loguje.</span><span class="sxs-lookup"><span data-stu-id="bfda6-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="bfda6-123">Zmiany strony\_załadować zdarzeń w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="bfda6-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="bfda6-124">Następnie Dodaj program obsługi zdarzeń "LoggedIn" następująco ustawiona nazwa sesji na nowo zalogowanego użytkownika i zmienić identyfikator tymczasowych sesji w koszyku do tego użytkownika, wywołując metodę MigrateCart w klasie naszych MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="bfda6-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="bfda6-125">(Zaimplementowano w pliku .cs)</span><span class="sxs-lookup"><span data-stu-id="bfda6-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="bfda6-126">Implementacja metody MigrateCart() następująco.</span><span class="sxs-lookup"><span data-stu-id="bfda6-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="bfda6-127">W checkout.aspx użyjemy EntityDataSource i GridView w naszym wyewidencjonowanie strony, na ile My mieliśmy w naszej stronie koszyka.</span><span class="sxs-lookup"><span data-stu-id="bfda6-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="bfda6-128">Należy pamiętać, że nasze kontrolki GridView określa procedurę obsługi zdarzeń "ondatabound" o nazwie MyList\_RowDataBound więc wykonania tego programu obsługi zdarzeń, takich jak to.</span><span class="sxs-lookup"><span data-stu-id="bfda6-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="bfda6-129">To Suma zakupów koszyka każdy wiersz jest powiązany i aktualizuje dolny wiersz GridView przechowuje metody.</span><span class="sxs-lookup"><span data-stu-id="bfda6-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="bfda6-130">Na tym etapie zaimplementowano rozwiązania tlp prezentacji "Recenzja" zlecenia do umieszczenia.</span><span class="sxs-lookup"><span data-stu-id="bfda6-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="bfda6-131">Umożliwia obsługę scenariusza pusty koszyka, dodając kilka wierszy kodu do strony\_załadowane zdarzenie:</span><span class="sxs-lookup"><span data-stu-id="bfda6-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="bfda6-132">Gdy użytkownik kliknie przycisk "Zatwierdź" Firma Microsoft będzie wykonaj następujący kod w obsłudze zdarzeń kliknij przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="bfda6-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="bfda6-133">"Rodzaje" proces przesyłania zamówienia jest realizowane w metodzie SubmitOrder() klasy Nasze MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="bfda6-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="bfda6-134">SubmitOrder wykonują następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="bfda6-134">SubmitOrder will:</span></span>

- <span data-ttu-id="bfda6-135">Wykonaj wszystkie pozycje w koszyku i używać ich do tworzenia nowego rekordu zlecenia i skojarzonych rekordów OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="bfda6-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="bfda6-136">Oblicz Data wysyłki.</span><span class="sxs-lookup"><span data-stu-id="bfda6-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="bfda6-137">Wyczyść koszyk sklepowy.</span><span class="sxs-lookup"><span data-stu-id="bfda6-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="bfda6-138">Na potrzeby tej przykładowej aplikacji będzie obliczania Data wysyłki, po prostu dodając dwa dni data bieżąca.</span><span class="sxs-lookup"><span data-stu-id="bfda6-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="bfda6-139">Uruchomiona jest aplikacja teraz pozwolą nam Przetestuj proces zakupów od początku do końca.</span><span class="sxs-lookup"><span data-stu-id="bfda6-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bfda6-140">[Poprzednie](tailspin-spyworks-part-5.md)
> [dalej](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="bfda6-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
