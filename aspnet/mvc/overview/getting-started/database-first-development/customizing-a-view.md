---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Baza danych EF najpierw o platformie ASP.NET MVC: dostosowywanie widok | Dokumentacja firmy Microsoft'
author: tfitzmac
description: Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 8338603e032329ad03d47c6392e508aa07c6858e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867661"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="5a1b7-104">Baza danych EF najpierw o platformie ASP.NET MVC: Dostosowywanie widoku</span><span class="sxs-lookup"><span data-stu-id="5a1b7-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="5a1b7-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5a1b7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5a1b7-106">Przy użyciu MVC, Entity Framework i szkieletów ASP.NET, można utworzyć aplikacji sieci web, która zapewnia interfejs do istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5a1b7-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="5a1b7-107">Ta seria samouczka przedstawiono sposób automatycznego generowania kodu, która umożliwia użytkownikom wyświetlanie, edytowanie, tworzenie i Usuń dane, które znajdują się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5a1b7-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="5a1b7-108">Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5a1b7-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="5a1b7-109">Ta część serii koncentruje się na zmienianie widoków automatycznie generowane w celu zwiększenia prezentacji.</span><span class="sxs-lookup"><span data-stu-id="5a1b7-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="5a1b7-110">Dodaj kursy zarejestrowanych do szczegółów dla użytkowników domowych</span><span class="sxs-lookup"><span data-stu-id="5a1b7-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="5a1b7-111">Wygenerowany kod zapewnia dobry punkt wyjścia dla aplikacji, ale nie zawsze zapewnia ona wszystkich funkcji, które są potrzebne w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a1b7-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="5a1b7-112">Można dostosować kodu w celu spełnienia wymagań konkretnej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a1b7-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="5a1b7-113">Obecnie aplikacji nie są wyświetlane szkoleń w zarejestrowany dla uczniów wybrane.</span><span class="sxs-lookup"><span data-stu-id="5a1b7-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="5a1b7-114">W tej sekcji dodasz kursy zarejestrowanych dla użytkowników do **szczegóły** widok studenta.</span><span class="sxs-lookup"><span data-stu-id="5a1b7-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="5a1b7-115">Otwórz **Students/Details.cshtml**i poniżej ostatniego &lt;/dl&gt; kartę, ale przed zamknięciem &lt;/DIV&gt; tagów, Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="5a1b7-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="5a1b7-116">Ten kod tworzy tabelę, która wyświetla wiersz dla każdego rekordu w tabeli rejestracji dla uczniów wybrane.</span><span class="sxs-lookup"><span data-stu-id="5a1b7-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="5a1b7-117">**Wyświetlania** metoda powoduje renderowanie kodu HTML dla obiekt (modelItem), który reprezentuje wyrażenie.</span><span class="sxs-lookup"><span data-stu-id="5a1b7-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="5a1b7-118">Metoda wyświetlania (zamiast po prostu osadzanie wartości właściwości w kodzie) do upewnij się, że wartość jest sformatowany poprawnie na podstawie typu i szablon dla tego typu.</span><span class="sxs-lookup"><span data-stu-id="5a1b7-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="5a1b7-119">W tym przykładzie każde wyrażenie zwraca jedną właściwość z bieżącego rekordu w pętli i wartości są typy pierwotne, które mają być renderowane jako tekst.</span><span class="sxs-lookup"><span data-stu-id="5a1b7-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="5a1b7-120">Przejdź do widoku studentów/indeksu ponownie, a następnie wybierz **szczegóły** jednego studentów.</span><span class="sxs-lookup"><span data-stu-id="5a1b7-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="5a1b7-121">Zobaczysz, że zarejestrowane kursy zostały uwzględnione w widoku.</span><span class="sxs-lookup"><span data-stu-id="5a1b7-121">You will see the enrolled courses have been included in the view.</span></span>

![uczniów przy rejestracji.](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="5a1b7-123">[Poprzednie](changing-the-database.md)
> [dalej](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="5a1b7-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
