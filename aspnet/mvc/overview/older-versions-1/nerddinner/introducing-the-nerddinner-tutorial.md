---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Wprowadzenie do samouczka NerdDinner | Dokumentacja firmy Microsoft
author: shanselman
description: Najlepszym sposobem, aby dowiedzieć się nowej struktury jest Zbuduj coś z nim. Ten samouczek zawiera szczegółowe instrukcje dotyczące tworzenia aplikacji mały, ale pełny, za pomocą ASP.NE...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 735a775752beda3fc24852422b1ff9d60a7c46b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371631"
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="3ce35-104">Wprowadzenie do samouczka NerdDinner</span><span class="sxs-lookup"><span data-stu-id="3ce35-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="3ce35-105">przez [Scotta Hanselmana](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="3ce35-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="3ce35-106">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="3ce35-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="3ce35-107">Najlepszym sposobem, aby dowiedzieć się nowej struktury jest Zbuduj coś z nim.</span><span class="sxs-lookup"><span data-stu-id="3ce35-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="3ce35-108">W tym samouczku przedstawiono sposób tworzenia małych, ale wykonania aplikacji za pomocą platformy ASP.NET MVC 1 oraz przedstawiono niektóre z podstawowych pojęciach związanych z nim.</span><span class="sxs-lookup"><span data-stu-id="3ce35-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="3ce35-109">Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.</span><span class="sxs-lookup"><span data-stu-id="3ce35-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="3ce35-110">Samouczka NerdDinner</span><span class="sxs-lookup"><span data-stu-id="3ce35-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="3ce35-111">Najlepszym sposobem, aby dowiedzieć się nowej struktury jest Zbuduj coś z nim.</span><span class="sxs-lookup"><span data-stu-id="3ce35-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="3ce35-112">W tym samouczku przedstawiono sposób tworzenia małych, ale wykonania aplikacji za pomocą platformy ASP.NET MVC i wprowadza niektóre z podstawowych pojęciach związanych z nim.</span><span class="sxs-lookup"><span data-stu-id="3ce35-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="3ce35-113">Aplikację, którą zamierzamy kompilacji jest nazywany "NerdDinner".</span><span class="sxs-lookup"><span data-stu-id="3ce35-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="3ce35-114">NerdDinner zapewnia łatwy sposób dla osób, Znajdź i organizowania kolacji w trybie online:</span><span class="sxs-lookup"><span data-stu-id="3ce35-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="3ce35-115">NerdDinner umożliwia tworzenie, edytowanie i usuwanie kolacji dla zarejestrowanych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="3ce35-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="3ce35-116">Wymusza on spójny zestaw reguł biznesowych i weryfikacji w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3ce35-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="3ce35-117">Osoby odwiedzające opartych na technologii AJAX tablica służy do wyszukiwania nadchodzących kolacji odbywają się w pobliżu je:</span><span class="sxs-lookup"><span data-stu-id="3ce35-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="3ce35-118">Klikając obiad powoduje wyświetlenie strony szczegółów gdzie oni dowiedzieć się więcej na ten temat:</span><span class="sxs-lookup"><span data-stu-id="3ce35-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="3ce35-119">Jeśli są zainteresowani udziałowi obiad mogą zalogować się lub zarejestrować się w lokacji:</span><span class="sxs-lookup"><span data-stu-id="3ce35-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="3ce35-120">Są następnie kliknąć link RSVP opartych na technologii AJAX w taki sposób, aby weź udział w wydarzeniu:</span><span class="sxs-lookup"><span data-stu-id="3ce35-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="3ce35-121">Implementowanie NerdDinner</span><span class="sxs-lookup"><span data-stu-id="3ce35-121">Implementing NerdDinner</span></span>

<span data-ttu-id="3ce35-122">Zamierzamy zacząć naszej aplikacji NerdDinner przy użyciu pliku -&gt;polecenie Nowy projekt w programie Visual Studio do tworzenia nowego projektu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3ce35-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="3ce35-123">Następnie przyrostowe dodamy funkcje i.</span><span class="sxs-lookup"><span data-stu-id="3ce35-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="3ce35-124">Po drodze omówimy:</span><span class="sxs-lookup"><span data-stu-id="3ce35-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="3ce35-125">Tworzenie nowego projektu MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3ce35-125">How to create a new ASP.NET MVC Project</span></span>](# "Utwórz nowy projekt ASP.NET MVC")
2. [<span data-ttu-id="3ce35-126">Jak utworzyć bazę danych</span><span class="sxs-lookup"><span data-stu-id="3ce35-126">How to create a database</span></span>](# "tworzenie bazy danych")
3. [<span data-ttu-id="3ce35-127">Sposób tworzenia modelu z weryfikacją reguł biznesowych</span><span class="sxs-lookup"><span data-stu-id="3ce35-127">How to build a model with business rule validations</span></span>](# "Budowanie modelu z weryfikacją reguł biznesowych")
4. [<span data-ttu-id="3ce35-128">Jak zaimplementować lista/szczegóły interfejsu użytkownika za pomocą widoków i kontrolerów</span><span class="sxs-lookup"><span data-stu-id="3ce35-128">How to use controllers and views to implement a listing/details UI</span></span>](# "używać kontrolery i widoki, do zaimplementowania interfejsu użytkownika lista/szczegóły")
5. <span data-ttu-id="3ce35-129">[Jak zapewnić CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) danych tworzą obsługi](# "obsługuje wpis formularza danych zapewniają CRUD (tworzenia, odczytu, Update, Delete)")</span><span class="sxs-lookup"><span data-stu-id="3ce35-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="3ce35-130">Jak używać ViewData i Implementowanie klas ViewModel</span><span class="sxs-lookup"><span data-stu-id="3ce35-130">How to use ViewData and implement ViewModel classes</span></span>](# "korzystać z podejścia ViewData i Implementowanie klas ViewModel")
7. [<span data-ttu-id="3ce35-131">Jak ponownie używać interfejsu użytkownika za pomocą stron wzorcowych i częściowych</span><span class="sxs-lookup"><span data-stu-id="3ce35-131">How to re-use UI using master pages and partials</span></span>](# "ponowne używanie interfejsu użytkownika za pomocą stron wzorcowych i częściowych")
8. [<span data-ttu-id="3ce35-132">Implementowanie wydajnego stronicowania danych jak</span><span class="sxs-lookup"><span data-stu-id="3ce35-132">How to implement efficient data paging</span></span>](# "zaimplementować wydajne danych stronicowania")
9. [<span data-ttu-id="3ce35-133">Jak zabezpieczyć aplikacje przy użyciu uwierzytelniania i autoryzacji</span><span class="sxs-lookup"><span data-stu-id="3ce35-133">How to secure applications using authentication and authorization</span></span>](# "bezpieczne aplikacje przy użyciu uwierzytelniania i autoryzacji")
10. [<span data-ttu-id="3ce35-134">Jak korzystanie z technologii AJAX w celu dostarczania aktualizacji dynamicznych</span><span class="sxs-lookup"><span data-stu-id="3ce35-134">How to use AJAX to deliver dynamic updates</span></span>](# "Użyj AJAX do dostarczania aktualizacji dynamicznych")
11. [<span data-ttu-id="3ce35-135">Sposób użycia interfejsu AJAX w celu implementacji scenariuszy mapowania</span><span class="sxs-lookup"><span data-stu-id="3ce35-135">How to use AJAX to implement mapping scenarios</span></span>](# "Użyj AJAX do implementacji scenariuszy mapowania")
12. [<span data-ttu-id="3ce35-136">Jak włączyć automatyczne testy jednostkowe</span><span class="sxs-lookup"><span data-stu-id="3ce35-136">How to enable automated unit testing</span></span>](# "Włącz zautomatyzowane testy jednostkowe")

<span data-ttu-id="3ce35-137">Możesz tworzyć własną kopię NerdDinner od podstaw, wykonując każdego kroku będziemy instrukcje przedstawione w tym rozdziale.</span><span class="sxs-lookup"><span data-stu-id="3ce35-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="3ce35-138">Alternatywnie możesz pobrać pełną wersję kodu źródłowego w tym miejscu: [NerdDinner w serwisie GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="3ce35-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="3ce35-139">Możesz również opcjonalnie również [Pobierz bezpłatną wersję PDF po ukończeniu tego samouczka](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) aby przeczytaj samouczek w trybie offline.</span><span class="sxs-lookup"><span data-stu-id="3ce35-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="3ce35-140">Aby skompilować aplikację, można użyć programu Visual Studio 2008 lub bezpłatnego programu Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="3ce35-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="3ce35-141">Dla bazy danych, można użyć programu SQL Server lub bezpłatnego programu SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="3ce35-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="3ce35-142">ASP.NET MVC, Visual Web Developer 2008 Express i programu SQL Server Express (wszystkie wersja bezpłatna) przy użyciu programu w wersji 2 można zainstalować [Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="3ce35-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="3ce35-143">Teraz Rozpocznijmy od...</span><span class="sxs-lookup"><span data-stu-id="3ce35-143">Now let's get started....</span></span>

<span data-ttu-id="3ce35-144">Teraz, gdy Omówiliśmy już jest NerdDinner, umożliwia rzutowanie naszych programistyczne firmy Microsoft i pisanie kodu.</span><span class="sxs-lookup"><span data-stu-id="3ce35-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="3ce35-145">Firma Microsoft rozpocznie się za pomocą pliku -&gt;nowy projekt w programie Visual Studio do tworzenia aplikacji NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="3ce35-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3ce35-146">Next</span><span class="sxs-lookup"><span data-stu-id="3ce35-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
