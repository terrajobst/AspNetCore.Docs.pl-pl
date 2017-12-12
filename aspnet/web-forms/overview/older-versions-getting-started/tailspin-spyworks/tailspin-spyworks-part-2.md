---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: "Część 2: Warstwa dostępu do danych | Dokumentacja firmy Microsoft"
author: JoeStagner
description: "Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 2 obejmuje dodawanie Warstwa dostępu do danych."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8b07b320640c1bb0074a4d3a04ca7c5b7e7bb6cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="part-2-data-access-layer"></a><span data-ttu-id="fe48f-104">Część 2: Warstwa dostępu do danych</span><span class="sxs-lookup"><span data-stu-id="fe48f-104">Part 2: Data Access Layer</span></span>
====================
<span data-ttu-id="fe48f-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="fe48f-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="fe48f-106">Tailspin Spyworks pokazano, jak bardzo proste jest tworzenie zaawansowanych, skalowalnych aplikacji dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="fe48f-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="fe48f-107">Przedstawia on poza jak nowe, fantastyczne funkcje programu ASP.NET 4 do tworzenia sklepu online, łącznie z zakupów, wyewidencjonowania i administracji.</span><span class="sxs-lookup"><span data-stu-id="fe48f-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="fe48f-108">Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fe48f-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="fe48f-109">Część 2 obejmuje dodawanie Warstwa dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="fe48f-109">Part 2 covers adding the data access layer.</span></span>


## <a id="_Toc260221668"></a><span data-ttu-id="fe48f-110">Dodawanie Warstwa dostępu do danych</span><span class="sxs-lookup"><span data-stu-id="fe48f-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="fe48f-111">Naszej aplikacji handlu elektronicznego będzie zależeć od dwóch baz danych.</span><span class="sxs-lookup"><span data-stu-id="fe48f-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="fe48f-112">Aby uzyskać informacje o kliencie użyjemy standardowe bazy danych członkostwa ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fe48f-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="fe48f-113">Naszym zakupów katalogu koszyka i produktu firma Microsoft będzie zaimplementować bazy danych SQL Express w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="fe48f-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="fe48f-114">Posiadanie utworzone bazy danych (Commerce.mdf) w aplikacji App\_folderu danych, można przejść do tworzenia naszych Warstwa dostępu do danych przy użyciu programu Entity Framework .NET.</span><span class="sxs-lookup"><span data-stu-id="fe48f-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="fe48f-115">Utworzymy folder o nazwie "danych\_dostępu" i ich kliknij folder prawym przyciskiem myszy i wybierz opcję "Dodaj nowy element".</span><span class="sxs-lookup"><span data-stu-id="fe48f-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="fe48f-116">W "Zainstalowane szablony" elementu, a następnie wybierz opcję "modelu danych jednostki ADO.NET" Wprowadź EDM\_Commerce.edmx jako nazwy i kliknij przycisk "Dodaj".</span><span class="sxs-lookup"><span data-stu-id="fe48f-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="fe48f-117">Wybierz pozycję "Generuj z bazy danych".</span><span class="sxs-lookup"><span data-stu-id="fe48f-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="fe48f-118">Zapisz i kompilacji.</span><span class="sxs-lookup"><span data-stu-id="fe48f-118">Save and build.</span></span>

<span data-ttu-id="fe48f-119">Teraz możemy dodać naszej pierwszej funkcji — menu kategorii produktów.</span><span class="sxs-lookup"><span data-stu-id="fe48f-119">Now we are ready to add our first feature – a product category menu.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="fe48f-120">[Poprzednie](tailspin-spyworks-part-1.md)
[dalej](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="fe48f-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
