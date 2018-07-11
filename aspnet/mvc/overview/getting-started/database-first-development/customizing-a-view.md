---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'EF bazy danych, najpierw z platformą ASP.NET MVC: Dostosowywanie widoku | Dokumentacja firmy Microsoft'
author: tfitzmac
description: Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: aspnetcontent
ms.date: 10/01/2014
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7359b8daddc74e375675d73126d7d76b288e853d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817191"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="5a330-104">EF bazy danych, najpierw z platformą ASP.NET MVC: Dostosowywanie widoku</span><span class="sxs-lookup"><span data-stu-id="5a330-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="5a330-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5a330-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5a330-106">Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5a330-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="5a330-107">W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5a330-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="5a330-108">Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5a330-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="5a330-109">Ta część serii koncentruje się na zmianę widoki generowane automatycznie, aby zwiększyć prezentacji.</span><span class="sxs-lookup"><span data-stu-id="5a330-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="5a330-110">Dodaj zarejestrowanych kursów do szczegółów dla uczniów</span><span class="sxs-lookup"><span data-stu-id="5a330-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="5a330-111">Wygenerowany kod zapewnia dobry punkt wyjścia dla twojej aplikacji, ale nie zawsze zapewnia ona wszystkich funkcji, które są potrzebne w Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a330-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="5a330-112">Można dostosować kod w celu spełnienia wymagań konkretnej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a330-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="5a330-113">Obecnie aplikację nie są wyświetlane zarejestrowanych kursy dla wybranej uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="5a330-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="5a330-114">W tej sekcji dodasz kursy zarejestrowanych dla każdego ucznia, aby **szczegóły** widoku dla uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="5a330-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="5a330-115">Otwórz **Students/Details.cshtml**, a poniżej ostatniej &lt;/dl&gt; karcie, ale przed zamykającym &lt;/DIV&gt; tag, Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="5a330-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="5a330-116">Ten kod tworzy tabelę, która wyświetla wiersz dla każdego rekordu w tabeli rejestracji dla wybranych uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="5a330-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="5a330-117">**Wyświetlania** metoda powoduje renderowanie kodu HTML dla obiektu (elementu modelu), który reprezentuje wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="5a330-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="5a330-118">Metoda wyświetlania (a nie po prostu osadzania wartości właściwości w kodzie), aby upewnić się, czy wartość jest sformatowana poprawnie na podstawie jego typu i szablon dla tego typu.</span><span class="sxs-lookup"><span data-stu-id="5a330-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="5a330-119">W tym przykładzie każde wyrażenie zwraca jedną właściwość z bieżącego rekordu w pętli, a wartości są typy pierwotne, które są renderowane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="5a330-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="5a330-120">Przejdź do widoku studentów/Index ponownie, a następnie wybierz pozycję **szczegóły** dla jednego z uczniów.</span><span class="sxs-lookup"><span data-stu-id="5a330-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="5a330-121">Zobaczysz, że zarejestrowane kursy zostały uwzględnione w widoku.</span><span class="sxs-lookup"><span data-stu-id="5a330-121">You will see the enrolled courses have been included in the view.</span></span>

![dla uczniów z rejestracją](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="5a330-123">[Poprzednie](changing-the-database.md)
> [dalej](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="5a330-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
