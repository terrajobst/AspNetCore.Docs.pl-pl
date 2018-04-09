---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Część 6: Członkostwo ASP.NET | Dokumentacja firmy Microsoft'
author: JoeStagner
description: Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 6 dodaje członkostwa ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 83e9bc780ea8face3e0f55fdf8c00e13b60f80a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="e839a-104">Część 6: Członkostwo ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e839a-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="e839a-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e839a-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e839a-106">Tailspin Spyworks pokazano, jak bardzo proste jest tworzenie zaawansowanych, skalowalnych aplikacji dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="e839a-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e839a-107">Przedstawia on poza jak nowe, fantastyczne funkcje programu ASP.NET 4 do tworzenia sklepu online, łącznie z zakupów, wyewidencjonowania i administracji.</span><span class="sxs-lookup"><span data-stu-id="e839a-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e839a-108">Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="e839a-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e839a-109">Część 6 dodaje członkostwa ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e839a-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a>  <span data-ttu-id="e839a-110">Praca z członkostwa ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e839a-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="e839a-111">Kliknij przycisk zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="e839a-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="e839a-112">Upewnij się, że użyto uwierzytelniania formularzy.</span><span class="sxs-lookup"><span data-stu-id="e839a-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="e839a-113">Umożliwia utworzenie kilku użytkowników link "Tworzenie użytkownika".</span><span class="sxs-lookup"><span data-stu-id="e839a-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="e839a-114">Na koniec można znaleźć w oknie Eksploratora rozwiązań i Odśwież widok.</span><span class="sxs-lookup"><span data-stu-id="e839a-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="e839a-115">Należy pamiętać, że ASPNETDB. Utworzono MDF poprawnie.</span><span class="sxs-lookup"><span data-stu-id="e839a-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="e839a-116">Ten plik zawiera tabele do obsługi usług platformy ASP.NET core, takich jak członkostwo.</span><span class="sxs-lookup"><span data-stu-id="e839a-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="e839a-117">Można teraz rozpocząć wdrażanie realizacji.</span><span class="sxs-lookup"><span data-stu-id="e839a-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="e839a-118">Rozpocznij od utworzenia strony CheckOut.aspx.</span><span class="sxs-lookup"><span data-stu-id="e839a-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="e839a-119">Na stronie CheckOut.aspx tylko powinny być dostępne dla użytkowników, którzy są zalogowani, firma Microsoft będzie ograniczanie dostępu do rejestrowane w przystawce Użytkownicy i Przekierowanie użytkowników, którzy nie jest zalogowany do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="e839a-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="e839a-120">W tym celu dodamy następujących sekcji konfiguracji naszych pliku web.config.</span><span class="sxs-lookup"><span data-stu-id="e839a-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="e839a-121">Szablon aplikacji formularzy sieci Web programu ASP.NET dodano sekcję uwierzytelniania do naszej pliku web.config i automatycznie ustanowić domyślną stronę logowania.</span><span class="sxs-lookup"><span data-stu-id="e839a-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="e839a-122">Firma Microsoft należy zmodyfikować plik do migracji anonimowe koszyk, gdy użytkownik loguje się kodzie Login.aspx.</span><span class="sxs-lookup"><span data-stu-id="e839a-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="e839a-123">Zmień stronę\_załadować zdarzeń w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="e839a-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="e839a-124">Następnie Dodaj program obsługi zdarzeń "LoggedIn" jak nazwa sesji do nowo zalogowanego użytkownika i zmień identyfikator sesji tymczasowe w koszyku do użytkownika, wywołując metodę MigrateCart w naszej klasy MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="e839a-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="e839a-125">(Zaimplementowana w pliku CS)</span><span class="sxs-lookup"><span data-stu-id="e839a-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="e839a-126">Implementacja metody MigrateCart() podobny.</span><span class="sxs-lookup"><span data-stu-id="e839a-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="e839a-127">W checkout.aspx użyjemy obiektu EntityDataSource i Element GridView w naszym wyewidencjonowanie strony podobnie NAS w naszej stronie koszyka.</span><span class="sxs-lookup"><span data-stu-id="e839a-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="e839a-128">Należy pamiętać, że nasze kontrolki widoku siatki Określa program obsługi zdarzeń "ondatabound" o nazwie MyList\_RowDataBound więc warto implementacji programu obsługi zdarzeń następująco.</span><span class="sxs-lookup"><span data-stu-id="e839a-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="e839a-129">Ten utrzymuje — metoda sumę zakupy koszyka każdy wiersz jest powiązany i aktualizuje dolnego wiersza widoku GridView.</span><span class="sxs-lookup"><span data-stu-id="e839a-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="e839a-130">Na tym etapie wdrożonych prezentacji "przeglądu" zlecenia do umieszczenia.</span><span class="sxs-lookup"><span data-stu-id="e839a-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="e839a-131">Załóżmy obsługi scenariusza pusty koszyka, dodając kilka wierszy kodu do strony\_zdarzeń obciążenia:</span><span class="sxs-lookup"><span data-stu-id="e839a-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="e839a-132">Gdy użytkownik kliknie przycisk "Zatwierdź" Firma Microsoft będzie wykonaj następujący kod do obsługi zdarzeń kliknij przycisk przesyłania.</span><span class="sxs-lookup"><span data-stu-id="e839a-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="e839a-133">"Rodzaje" proces przesyłania zamówienia jest realizowane w metodzie SubmitOrder() klasy Nasze MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="e839a-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="e839a-134">SubmitOrder następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="e839a-134">SubmitOrder will:</span></span>

- <span data-ttu-id="e839a-135">Wykonaj wszystkie pozycje w koszyku i używać ich do tworzenia nowego rekordu zlecenia i skojarzonych rekordów SzczegółyZamówień.</span><span class="sxs-lookup"><span data-stu-id="e839a-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="e839a-136">Oblicz Data wysyłki.</span><span class="sxs-lookup"><span data-stu-id="e839a-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="e839a-137">Wyczyść koszyk.</span><span class="sxs-lookup"><span data-stu-id="e839a-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="e839a-138">Na potrzeby tej przykładowej aplikacji firma Microsoft będzie obliczać Data wysyłki po prostu dodając dwa dni do daty bieżącej.</span><span class="sxs-lookup"><span data-stu-id="e839a-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="e839a-139">Uruchamianie aplikacji teraz pozwolą firmie Microsoft w celu przetestowania zakupów procesu od początku do końca.</span><span class="sxs-lookup"><span data-stu-id="e839a-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e839a-140">[Poprzednie](tailspin-spyworks-part-5.md)
> [dalej](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="e839a-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
