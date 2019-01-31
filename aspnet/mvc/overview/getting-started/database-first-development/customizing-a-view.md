---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Samouczek: Dostosowywanie widoku dla platformy EF Database First w aplikacji ASP.NET MVC'
description: Ten samouczek koncentruje się na zmianę widoki generowane automatycznie, aby zwiększyć prezentacji.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236500"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="8814a-103">Samouczek: Dostosowywanie widoku dla platformy EF Database First w aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8814a-103">Tutorial: Customize view for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="8814a-104">Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8814a-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="8814a-105">W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8814a-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="8814a-106">Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8814a-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="8814a-107">Ten samouczek koncentruje się na zmianę widoki generowane automatycznie, aby zwiększyć prezentacji.</span><span class="sxs-lookup"><span data-stu-id="8814a-107">This tutorial focuses on changing the automatically-generated views to enhance the presentation.</span></span>

<span data-ttu-id="8814a-108">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="8814a-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8814a-109">Dodaj kursy do strony szczegółów dla uczniów</span><span class="sxs-lookup"><span data-stu-id="8814a-109">Add courses to the student detail page</span></span>
> * <span data-ttu-id="8814a-110">Upewnij się, że kursy są dodawane do strony</span><span class="sxs-lookup"><span data-stu-id="8814a-110">Confirm that the courses are added to the page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8814a-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="8814a-111">Prerequisites</span></span>

* [<span data-ttu-id="8814a-112">Zmień bazę danych</span><span class="sxs-lookup"><span data-stu-id="8814a-112">Change the database</span></span>](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a><span data-ttu-id="8814a-113">Dodaj kursy do szczegóły dotyczące ucznia</span><span class="sxs-lookup"><span data-stu-id="8814a-113">Add courses to student detail</span></span>

<span data-ttu-id="8814a-114">Wygenerowany kod zapewnia dobry punkt wyjścia dla twojej aplikacji, ale nie zawsze zapewnia ona wszystkich funkcji, które są potrzebne w Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8814a-114">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="8814a-115">Można dostosować kod w celu spełnienia wymagań konkretnej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8814a-115">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="8814a-116">Obecnie aplikację nie są wyświetlane zarejestrowanych kursy dla wybranej uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="8814a-116">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="8814a-117">W tej sekcji dodasz kursy zarejestrowanych dla każdego ucznia, aby **szczegóły** widoku dla uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="8814a-117">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="8814a-118">Otwórz **widoków** > **studentów** > *Details.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8814a-118">Open **Views** > **Students** > *Details.cshtml*.</span></span> <span data-ttu-id="8814a-119">Poniżej ostatniej &lt;/dl&gt; tagu, ale przed zamykającym &lt;/DIV&gt; tag, Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="8814a-119">Below the last &lt;/dl&gt; tag, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="8814a-120">Ten kod tworzy tabelę, która wyświetla wiersz dla każdego rekordu w tabeli rejestracji dla wybranych uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="8814a-120">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="8814a-121">**Wyświetlania** metoda powoduje renderowanie kodu HTML dla obiektu (elementu modelu), który reprezentuje wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="8814a-121">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="8814a-122">Metoda wyświetlania (a nie po prostu osadzania wartości właściwości w kodzie), aby upewnić się, czy wartość jest sformatowana poprawnie na podstawie jego typu i szablon dla tego typu.</span><span class="sxs-lookup"><span data-stu-id="8814a-122">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="8814a-123">W tym przykładzie każde wyrażenie zwraca jedną właściwość z bieżącego rekordu w pętli, a wartości są typy pierwotne, które są renderowane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="8814a-123">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

## <a name="confirm-courses-are-added"></a><span data-ttu-id="8814a-124">Upewnij się, że są dodawane kursów</span><span class="sxs-lookup"><span data-stu-id="8814a-124">Confirm courses are added</span></span>

<span data-ttu-id="8814a-125">Uruchom rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="8814a-125">Run the solution.</span></span> <span data-ttu-id="8814a-126">Kliknij przycisk **listę uczniów** i wybierz **szczegóły** dla jednego z uczniów.</span><span class="sxs-lookup"><span data-stu-id="8814a-126">Click **List of students** and select **Details** for one of the students.</span></span> <span data-ttu-id="8814a-127">Zobaczysz, że zarejestrowane kursy zostały uwzględnione w widoku.</span><span class="sxs-lookup"><span data-stu-id="8814a-127">You will see the enrolled courses have been included in the view.</span></span>

![dla uczniów z rejestracją](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a><span data-ttu-id="8814a-129">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="8814a-129">Next steps</span></span>
<span data-ttu-id="8814a-130">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="8814a-130">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8814a-131">Dodano kursy do strony szczegółów dla uczniów</span><span class="sxs-lookup"><span data-stu-id="8814a-131">Added courses to the student detail page</span></span>
> * <span data-ttu-id="8814a-132">Potwierdza, że kursy są dodawane do strony</span><span class="sxs-lookup"><span data-stu-id="8814a-132">Confirmed that the courses are added to the page</span></span>

<span data-ttu-id="8814a-133">Przejdź do następnego samouczka, aby dowiedzieć się, jak dodawać adnotacje danych do określania wymagań dotyczących walidacji i wyświetlenie formatowania.</span><span class="sxs-lookup"><span data-stu-id="8814a-133">Advance to the next tutorial to learn how to add data annotations to specify validation requirements and display formatting.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="8814a-134">Ulepszanie weryfikacji danych</span><span class="sxs-lookup"><span data-stu-id="8814a-134">Enhance data validation</span></span>](enhancing-data-validation.md)
