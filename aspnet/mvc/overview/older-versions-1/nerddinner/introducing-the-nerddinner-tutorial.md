---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Wprowadzenie do samouczka NerdDinner | Dokumentacja firmy Microsoft
author: shanselman
description: Najlepszym sposobem poznawania nowej struktury jest z nim coś kompilacji. W tym samouczku przedstawiono sposób tworzenia mały, ale pełny, aplikacji, za pomocą ASP.NE...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 3d925a7dc89fc0c742468653c5c138a0f1d71231
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="40f37-104">Wprowadzenie do samouczka NerdDinner</span><span class="sxs-lookup"><span data-stu-id="40f37-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="40f37-105">przez [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="40f37-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="40f37-106">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="40f37-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="40f37-107">Najlepszym sposobem poznawania nowej struktury jest z nim coś kompilacji.</span><span class="sxs-lookup"><span data-stu-id="40f37-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="40f37-108">W tym samouczku przedstawiono sposób tworzenia małą, ale zakończyć aplikację przy użyciu platformy ASP.NET MVC 1 i wprowadza niektóre podstawowe koncepcje za nią.</span><span class="sxs-lookup"><span data-stu-id="40f37-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="40f37-109">Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.</span><span class="sxs-lookup"><span data-stu-id="40f37-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="40f37-110">Samouczek NerdDinner</span><span class="sxs-lookup"><span data-stu-id="40f37-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="40f37-111">Najlepszym sposobem poznawania nowej struktury jest z nim coś kompilacji.</span><span class="sxs-lookup"><span data-stu-id="40f37-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="40f37-112">W tym samouczku przedstawiono sposób tworzenia małą, ale zakończyć aplikację przy użyciu platformy ASP.NET MVC i wprowadza niektóre podstawowe koncepcje za nią.</span><span class="sxs-lookup"><span data-stu-id="40f37-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="40f37-113">Aplikacja, któremu zamierzamy utworzyć nazywa się "NerdDinner".</span><span class="sxs-lookup"><span data-stu-id="40f37-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="40f37-114">NerdDinner zapewnia prosty sposób użytkownikom wyszukiwanie i organizowanie kolacji w trybie online:</span><span class="sxs-lookup"><span data-stu-id="40f37-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="40f37-115">NerdDinner umożliwia tworzenie, edytowanie i usuwanie kolacji dla zarejestrowanych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="40f37-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="40f37-116">Wymusza spójny zestaw reguł sprawdzania poprawności i biznesowych w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="40f37-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="40f37-117">Osoby odwiedzające opartych na technologii AJAX tablica służy do wyszukiwania nadchodzących kolacji odbywają się obok nich:</span><span class="sxs-lookup"><span data-stu-id="40f37-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="40f37-118">Kliknięcie przycisku obiad powoduje wyświetlenie strony szczegółów gdzie można znaleźć więcej informacji:</span><span class="sxs-lookup"><span data-stu-id="40f37-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="40f37-119">Jeśli są zainteresowani uczestniczący obiad mogą zalogować się lub zarejestrować się w lokacji:</span><span class="sxs-lookup"><span data-stu-id="40f37-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="40f37-120">One następnie kliknij łącze RSVP opartych na technologii AJAX do udziału zdarzenia:</span><span class="sxs-lookup"><span data-stu-id="40f37-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="40f37-121">Implementowanie NerdDinner</span><span class="sxs-lookup"><span data-stu-id="40f37-121">Implementing NerdDinner</span></span>

<span data-ttu-id="40f37-122">Zamierzamy rozpocząć naszej aplikacji NerdDinner przy użyciu pliku -&gt;polecenie Nowy projekt w programie Visual Studio do tworzenia nowego projektu platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="40f37-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="40f37-123">Następnie przyrostowo dodamy możliwości i funkcje.</span><span class="sxs-lookup"><span data-stu-id="40f37-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="40f37-124">Wzdłuż sposób omówione zostaną następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="40f37-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="40f37-125">Jak utworzyć nowy projekt ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="40f37-125">How to create a new ASP.NET MVC Project</span></span>](# "Utwórz nowy projekt ASP.NET MVC")
2. [<span data-ttu-id="40f37-126">Jak utworzyć bazę danych</span><span class="sxs-lookup"><span data-stu-id="40f37-126">How to create a database</span></span>](# "tworzenie bazy danych")
3. [<span data-ttu-id="40f37-127">Sposób tworzenia modelu sprawdzaniem poprawności reguły biznesowej</span><span class="sxs-lookup"><span data-stu-id="40f37-127">How to build a model with business rule validations</span></span>](# "Budowanie modelu sprawdzaniem poprawności reguły biznesowej")
4. [<span data-ttu-id="40f37-128">Jak używać kontrolery i widoki do zaimplementowania listę/szczegółów interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="40f37-128">How to use controllers and views to implement a listing/details UI</span></span>](# "używać kontrolery i widoki, do zaimplementowania interfejsu użytkownika listę/szczegółów")
5. <span data-ttu-id="40f37-129">[Jak zapewnić CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) danych tworzą Obsługa wpis](# "obsługuje wpis formularza danych Podaj CRUD (tworzenia, odczytu, aktualizacji, usuwania)")</span><span class="sxs-lookup"><span data-stu-id="40f37-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="40f37-130">Jak używać ViewData i implementuje klasy ViewModel</span><span class="sxs-lookup"><span data-stu-id="40f37-130">How to use ViewData and implement ViewModel classes</span></span>](# "Użyj ViewData i implementuje klasy ViewModel")
7. [<span data-ttu-id="40f37-131">Jak ponownie użyć interfejsu użytkownika przy użyciu stron wzorcowych i częściowe</span><span class="sxs-lookup"><span data-stu-id="40f37-131">How to re-use UI using master pages and partials</span></span>](# "ponownego użycia interfejsu użytkownika przy użyciu stron wzorcowych i częściowe")
8. [<span data-ttu-id="40f37-132">Implementowania stronicowania danych wydajne</span><span class="sxs-lookup"><span data-stu-id="40f37-132">How to implement efficient data paging</span></span>](# "zaimplementować wydajne danych stronicowania")
9. [<span data-ttu-id="40f37-133">Jak zabezpieczyć aplikacji przy użyciu uwierzytelniania i autoryzacji</span><span class="sxs-lookup"><span data-stu-id="40f37-133">How to secure applications using authentication and authorization</span></span>](# "bezpiecznego aplikacji przy użyciu uwierzytelniania i autoryzacji")
10. [<span data-ttu-id="40f37-134">Sposób użycia interfejsu AJAX do dostarczenia aktualizacji dynamicznej</span><span class="sxs-lookup"><span data-stu-id="40f37-134">How to use AJAX to deliver dynamic updates</span></span>](# "używać technologii AJAX, aby dostarczać aktualizacje dynamiczne")
11. [<span data-ttu-id="40f37-135">Jak używać technologii AJAX do implementacji mapowania scenariusze</span><span class="sxs-lookup"><span data-stu-id="40f37-135">How to use AJAX to implement mapping scenarios</span></span>](# "Użyj AJAX do implementacji mapowania scenariuszy")
12. [<span data-ttu-id="40f37-136">Jak włączyć automatyczne testy jednostkowe</span><span class="sxs-lookup"><span data-stu-id="40f37-136">How to enable automated unit testing</span></span>](# "włączenia zautomatyzowanych testów jednostkowych")

<span data-ttu-id="40f37-137">Możesz utworzyć własną kopię NerdDinner od początku, wykonując każdego kroku będziemy wskazówki w tym rozdziale.</span><span class="sxs-lookup"><span data-stu-id="40f37-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="40f37-138">Alternatywnie możesz pobrać ukończoną wersję kodu źródłowego w tym miejscu: [NerdDinner w serwisie GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="40f37-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="40f37-139">Możesz również opcjonalnie również [Pobierz bezpłatną wersję PDF tego samouczka](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) Aby odczytać samouczek w trybie offline.</span><span class="sxs-lookup"><span data-stu-id="40f37-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="40f37-140">Do skompilowania aplikacji, można użyć programu Visual Studio 2008 lub wolnego Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="40f37-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="40f37-141">SQL Server lub wolnego programu SQL Server Express można użyć dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="40f37-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="40f37-142">Można zainstalować programu ASP.NET MVC, Visual Web Developer 2008 Express i programu SQL Server Express (wszystkim jest bezpłatny) przy użyciu V2 z [Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="40f37-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="40f37-143">Teraz do dzieła...</span><span class="sxs-lookup"><span data-stu-id="40f37-143">Now let's get started....</span></span>

<span data-ttu-id="40f37-144">Teraz, kiedy możemy zostały objęte jest NerdDinner, umożliwia rzutowanie naszych rękawami i pisania kodu.</span><span class="sxs-lookup"><span data-stu-id="40f37-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="40f37-145">Rozpocznie się za pomocą pliku -&gt;nowy projekt w programie Visual Studio, aby utworzyć aplikację NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="40f37-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="40f37-146">Next</span><span class="sxs-lookup"><span data-stu-id="40f37-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
