---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Część 2: Warstwa dostępu do danych | Dokumentacja firmy Microsoft'
author: JoeStagner
description: W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 2 obejmuje dodawanie warstwy dostępu do danych.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: c1d61681952cbf1f0587f0e42da21f8436d79ebb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812035"
---
<a name="part-2-data-access-layer"></a><span data-ttu-id="5826a-104">Część 2: Warstwa dostępu do danych</span><span class="sxs-lookup"><span data-stu-id="5826a-104">Part 2: Data Access Layer</span></span>
====================
<span data-ttu-id="5826a-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="5826a-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="5826a-106">Tailspin Spyworks pokazuje, jak bardzo łatwo jest tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="5826a-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="5826a-107">Przedstawia on poza sposób użycia wspaniałych nowych funkcjach w ASP.NET 4 do tworzenia sklep online, m.in. zakupy wyewidencjonowanie i Administracja.</span><span class="sxs-lookup"><span data-stu-id="5826a-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="5826a-108">W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="5826a-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="5826a-109">Część 2 obejmuje dodawanie warstwy dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="5826a-109">Part 2 covers adding the data access layer.</span></span>


## <a id="_Toc260221668"></a>  <span data-ttu-id="5826a-110">Dodawanie warstwy dostępu do danych</span><span class="sxs-lookup"><span data-stu-id="5826a-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="5826a-111">Nasza aplikacja handlu elektronicznego będzie zależeć od dwóch baz danych.</span><span class="sxs-lookup"><span data-stu-id="5826a-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="5826a-112">Aby uzyskać informacje o kliencie użyjemy standardowa baza danych członkostwa ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5826a-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="5826a-113">Dla wykazu zakupów koszyka i produktu będzie wdrażamy bazy danych programu SQL Express w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="5826a-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="5826a-114">Jeśli utworzono bazę danych (Commerce.mdf) w aplikacji App\_folder dane możemy rozpocząć tworzenie warstwy dostępu do danych, nasze używający narzędzia .NET Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5826a-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="5826a-115">Utworzymy folder o nazwie "danych\_dostępu" i je w tym folderze kliknij prawym przyciskiem myszy i wybierz pozycję "Dodaj nowy element".</span><span class="sxs-lookup"><span data-stu-id="5826a-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="5826a-116">W "Zainstalowane szablony" elementu, a następnie wybierz pozycję "ADO.NET Entity Data Model" Wprowadź EDM\_Commerce.edmx jako nazwę i kliknij przycisk "Dodaj".</span><span class="sxs-lookup"><span data-stu-id="5826a-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="5826a-117">Wybierz pozycję "Generuj z bazy danych".</span><span class="sxs-lookup"><span data-stu-id="5826a-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="5826a-118">Zapisz i kompilacji.</span><span class="sxs-lookup"><span data-stu-id="5826a-118">Save and build.</span></span>

<span data-ttu-id="5826a-119">Teraz jesteśmy gotowi dodać naszej pierwszej funkcji — menu kategorii produktów.</span><span class="sxs-lookup"><span data-stu-id="5826a-119">Now we are ready to add our first feature – a product category menu.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5826a-120">[Poprzednie](tailspin-spyworks-part-1.md)
> [dalej](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="5826a-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
