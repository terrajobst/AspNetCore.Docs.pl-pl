---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Wprowadzenie do formularzy sieci Web 4.5 ASP.NET i Visual Studio 2013 | Dokumentacja firmy Microsoft
author: Erikre
description: Ten krok samouczka serii uczy podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: a3527b54d1936bc14e32a1828ac3a2be625107ba
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/28/2018
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="5060c-103">Wprowadzenie do formularzy sieci Web 4.5 ASP.NET i Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5060c-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="5060c-104">Przez [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="5060c-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="5060c-105">[Pobierz Wingtip Toys przykładowy projekt (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="5060c-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="5060c-106">Ten krok samouczka serii nauczyć podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web.</span><span class="sxs-lookup"><span data-stu-id="5060c-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="5060c-107">Kwizu formularzy sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5060c-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="5060c-108">Sprawdź swoją wiedzę i wzmocnienie kluczowe założenia wykonując quizu formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5060c-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="5060c-109">Kwizu zostało zaprojektowane specjalnie z zawartości zawarte w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="5060c-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="5060c-110">Każde pytanie w kwizu zawiera wyjaśnienie oraz linki do dodatkowych wskazówek.</span><span class="sxs-lookup"><span data-stu-id="5060c-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="5060c-111">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="5060c-111">Introduction</span></span>

<span data-ttu-id="5060c-112">Tej serii samouczków prowadzi użytkownika przez kroki wymagane do tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu Visual Studio Express 2013 dla sieci Web i ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="5060c-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="5060c-113">Nosi nazwę aplikacji, należy utworzyć **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="5060c-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="5060c-114">Jest uproszczony przykład magazynu frontonu witryny sieci web, która sprzedaje elementów w trybie online.</span><span class="sxs-lookup"><span data-stu-id="5060c-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="5060c-115">Ten samouczek serii prezentuje nowych funkcji dostępnych w programie ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="5060c-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="5060c-116">Komentarze są powitalnej i wybierzemy wszelkich starań, aby zaktualizować ten samouczek serii oparta na sugestii.</span><span class="sxs-lookup"><span data-stu-id="5060c-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="5060c-117">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="5060c-117">Download completed project</span></span>

<span data-ttu-id="5060c-118">Możesz pobrać projekt C#, który zawiera samouczek ukończone.</span><span class="sxs-lookup"><span data-stu-id="5060c-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="5060c-119">[Wprowadzenie do formularzy sieci Web 4.5 ASP.NET i Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="5060c-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="5060c-120">Przejrzyj zawartość, wykonując pokrewne kwizu formularzy sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5060c-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="5060c-121">Po ukończeniu tego samouczka, sprawdź swoją wiedzę i wzmocnienia podstawowych pojęć, wykonując [ASP.NET Web Forms quizu](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="5060c-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="5060c-122">Kwizu zostało zaprojektowane specjalnie z zawartości zawarte w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="5060c-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="5060c-123">Każde pytanie w kwizu zawiera wyjaśnienie oraz linki do dodatkowych wskazówek.</span><span class="sxs-lookup"><span data-stu-id="5060c-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="5060c-124">Kwizu formularzy sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5060c-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="5060c-125">Odbiorcy</span><span class="sxs-lookup"><span data-stu-id="5060c-125">Audience</span></span>

<span data-ttu-id="5060c-126">Docelowa grupa odbiorców tego samouczka serii jest doświadczonych projektantów, którzy dopiero zaczynasz korzystać z formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5060c-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="5060c-127">Projektant zainteresowane w tym samouczku powinny mieć następujące umiejętności:</span><span class="sxs-lookup"><span data-stu-id="5060c-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="5060c-128">Dobrze znany z obiektem zorientowane na język programowania (Obiektowo)</span><span class="sxs-lookup"><span data-stu-id="5060c-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="5060c-129">Znasz koncepcji Projektowanie sieci Web (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="5060c-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="5060c-130">Znasz koncepcji relacyjnej bazy danych</span><span class="sxs-lookup"><span data-stu-id="5060c-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="5060c-131">Znasz koncepcji architektura n warstwowa</span><span class="sxs-lookup"><span data-stu-id="5060c-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="5060c-132">Jeśli jesteś zrecenzować obszarów wymienionych powyżej, należy wziąć pod uwagę zapoznaniu się z następującą zawartością:</span><span class="sxs-lookup"><span data-stu-id="5060c-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="5060c-133">Wprowadzenie do języka Visual C#</span><span class="sxs-lookup"><span data-stu-id="5060c-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="5060c-134">[Wdrażanie sieci Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="5060c-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="5060c-135">Relacyjna baza danych</span><span class="sxs-lookup"><span data-stu-id="5060c-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="5060c-136">Architektura wielowarstwowa</span><span class="sxs-lookup"><span data-stu-id="5060c-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="5060c-137">Funkcje aplikacji</span><span class="sxs-lookup"><span data-stu-id="5060c-137">Application Features</span></span>

<span data-ttu-id="5060c-138">Funkcje formularza sieci Web ASP.NET przedstawionych w tej serii obejmują:</span><span class="sxs-lookup"><span data-stu-id="5060c-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="5060c-139">Projekt aplikacji sieci Web (nie projekt witryny sieci Web)</span><span class="sxs-lookup"><span data-stu-id="5060c-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="5060c-140">Formularze sieci Web</span><span class="sxs-lookup"><span data-stu-id="5060c-140">Web Forms</span></span>
- <span data-ttu-id="5060c-141">Strony wzorcowe, konfiguracji</span><span class="sxs-lookup"><span data-stu-id="5060c-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="5060c-142">Ładowania początkowego</span><span class="sxs-lookup"><span data-stu-id="5060c-142">Bootstrap</span></span>
- <span data-ttu-id="5060c-143">Entity Framework Code najpierw LocalDB</span><span class="sxs-lookup"><span data-stu-id="5060c-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="5060c-144">Sprawdzanie poprawności żądań</span><span class="sxs-lookup"><span data-stu-id="5060c-144">Request Validation</span></span>
- <span data-ttu-id="5060c-145">Silnie Typizowane formantów danych wiązań adnotacji danych modelu i dostawców wartości</span><span class="sxs-lookup"><span data-stu-id="5060c-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="5060c-146">Protokół SSL i uwierzytelniania OAuth</span><span class="sxs-lookup"><span data-stu-id="5060c-146">SSL and OAuth</span></span>
- <span data-ttu-id="5060c-147">Tożsamość platformy ASP.NET, konfiguracji i autoryzacji</span><span class="sxs-lookup"><span data-stu-id="5060c-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="5060c-148">Sprawdzania poprawności dyskretnego kodu</span><span class="sxs-lookup"><span data-stu-id="5060c-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="5060c-149">Routing</span><span class="sxs-lookup"><span data-stu-id="5060c-149">Routing</span></span>
- <span data-ttu-id="5060c-150">Obsługa błędów ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5060c-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="5060c-151">Aplikacja scenariuszy i zadań</span><span class="sxs-lookup"><span data-stu-id="5060c-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="5060c-152">Zadania zostało to pokazane w tej serii obejmują:</span><span class="sxs-lookup"><span data-stu-id="5060c-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="5060c-153">Tworzenie, przeglądanie i uruchamianie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="5060c-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="5060c-154">Tworzenie struktury bazy danych</span><span class="sxs-lookup"><span data-stu-id="5060c-154">Creating the database structure</span></span>
- <span data-ttu-id="5060c-155">Inicjowanie i wstępne wypełnianie bazy danych</span><span class="sxs-lookup"><span data-stu-id="5060c-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="5060c-156">Dostosowywanie interfejsu użytkownika przy użyciu stylów, grafiki i strony wzorcowej</span><span class="sxs-lookup"><span data-stu-id="5060c-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="5060c-157">Dodawanie stron i nawigacji</span><span class="sxs-lookup"><span data-stu-id="5060c-157">Adding pages and navigation</span></span>
- <span data-ttu-id="5060c-158">Wyświetlanie szczegółów menu i dane produktu</span><span class="sxs-lookup"><span data-stu-id="5060c-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="5060c-159">Tworzenie koszyk</span><span class="sxs-lookup"><span data-stu-id="5060c-159">Creating a shopping cart</span></span>
- <span data-ttu-id="5060c-160">Obsługa dodawania SSL i uwierzytelniania OAuth</span><span class="sxs-lookup"><span data-stu-id="5060c-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="5060c-161">Dodawanie metody płatności</span><span class="sxs-lookup"><span data-stu-id="5060c-161">Adding a payment method</span></span>
- <span data-ttu-id="5060c-162">W tym rolę administratora i użytkownika do aplikacji</span><span class="sxs-lookup"><span data-stu-id="5060c-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="5060c-163">Ograniczanie dostępu do określonych stron i folderów</span><span class="sxs-lookup"><span data-stu-id="5060c-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="5060c-164">Przekazywanie pliku do aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="5060c-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="5060c-165">Implementowanie sprawdzania poprawności danych wejściowych</span><span class="sxs-lookup"><span data-stu-id="5060c-165">Implementing input validation</span></span>
- <span data-ttu-id="5060c-166">Rejestrowanie tras dla aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="5060c-166">Registering routes for the web application</span></span>
- <span data-ttu-id="5060c-167">Implementowanie obsługi błędów i rejestrowania błędów</span><span class="sxs-lookup"><span data-stu-id="5060c-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="5060c-168">Omówienie</span><span class="sxs-lookup"><span data-stu-id="5060c-168">Overview</span></span>

<span data-ttu-id="5060c-169">Jeśli jesteś nowym użytkownikiem programu ASP.NET Web Forms ale ma znajomość koncepcje programowania, masz prawo samouczka.</span><span class="sxs-lookup"><span data-stu-id="5060c-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="5060c-170">Jeśli już znasz formularzy sieci Web ASP.NET, mogą korzystać z tego samouczka serii przez nowych funkcji dostępnych w programie ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="5060c-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="5060c-171">Jeśli znasz programowania pojęcia i formularzy sieci Web ASP.NET, zobacz dodatkowe samouczki, podany w formularzach sieci Web [wprowadzenie](../../../index.md) sekcji w witrynie sieci Web programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5060c-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="5060c-172">Konkretnym **najnowsze** ASP.NET 4.5 funkcje wprowadzone w formularzach sieci Web, ten samouczek serii są następujące:</span><span class="sxs-lookup"><span data-stu-id="5060c-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="5060c-173">Prosty interfejs użytkownika służący do tworzenia projektów tej oferty [obsługę wielu platform ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC i interfejsu API sieci Web).</span><span class="sxs-lookup"><span data-stu-id="5060c-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="5060c-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), układ i motywów platforma, która zapewnia dynamiczne możliwości projektowania i tworzenia motywów.</span><span class="sxs-lookup"><span data-stu-id="5060c-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="5060c-175">[ASP.NET Identity](../../../../identity/index.md), nowego systemu członkostwa programu ASP.NET, który działa tak samo we wszystkich platform ASP.NET i współpracuje z oprogramowania innego niż IIS hostingu w sieci web.</span><span class="sxs-lookup"><span data-stu-id="5060c-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="5060c-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), aktualizacja do narzędzia Entity Framework, dzięki czemu możesz pobrać i manipulowanie danymi jako silnie typizowanych obiektów, uzyskiwanie dostępu do danych asynchronicznie, obsługi błędów przejściowych połączenia i dziennika instrukcji SQL.</span><span class="sxs-lookup"><span data-stu-id="5060c-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="5060c-177">Aby uzyskać pełną listę funkcji ASP.NET 4.5, zobacz [ASP.NET i narzędzia sieci Web dla programu Visual Studio 2013 informacje o wersji](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="5060c-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="5060c-178">Wingtip Toys przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="5060c-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="5060c-179">Poniższe zrzuty ekranu zapewniają szybki przegląd aplikacji formularzy sieci Web ASP.NET, która zostanie utworzona w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="5060c-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="5060c-180">Po uruchomieniu aplikacji z programu Visual Studio Express 2013 for Web zobaczą następującą stronę główną.</span><span class="sxs-lookup"><span data-stu-id="5060c-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Wingtip Toys — domyślna strona](introduction-and-overview/_static/image1.png)

<span data-ttu-id="5060c-182">Zarejestruj się jako nowy użytkownik lub zaloguj się jako istniejącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5060c-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="5060c-183">Nawigacji podano u góry dla każdej kategorii produktów, pobierając dostępnych produktów z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5060c-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="5060c-184">Po wybraniu łącza produktów, można wyświetlić listę wszystkich dostępnych produktów.</span><span class="sxs-lookup"><span data-stu-id="5060c-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Wingtip Toys - produktów](introduction-and-overview/_static/image2.png)

<span data-ttu-id="5060c-186">Można również wyświetlić szczegółowe informacje indywidualnych produktów, wybierając jedną z wymienionych produktów.</span><span class="sxs-lookup"><span data-stu-id="5060c-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Wingtip Toys — szczegółowe informacje o produkcie](introduction-and-overview/_static/image3.png)

<span data-ttu-id="5060c-188">Użytkownik może rejestrować i zaloguj się za pomocą funkcji domyślnego szablonu formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="5060c-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="5060c-189">W tym samouczku wyjaśniono również sposób zalogować się przy użyciu istniejącego konta usługi Gmail.</span><span class="sxs-lookup"><span data-stu-id="5060c-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="5060c-190">Ponadto możesz zalogować się jako administrator, aby dodawać i usuwać produkty z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5060c-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Zaloguj się do niego Wingtip Toys-](introduction-and-overview/_static/image4.png)

<span data-ttu-id="5060c-192">Gdy użytkownik zalogował się jako użytkownik, można dodać produktów koszyk i wyewidencjonowania z PayPal.</span><span class="sxs-lookup"><span data-stu-id="5060c-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="5060c-193">Zauważ, że ta Przykładowa aplikacja jest działa w programie PayPal developer piaskownicy.</span><span class="sxs-lookup"><span data-stu-id="5060c-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="5060c-194">Żadna transakcja rzeczywiste pieniędzy będą miały miejsce.</span><span class="sxs-lookup"><span data-stu-id="5060c-194">No actual money transaction will take place.</span></span>

![Wingtip Toys - koszyka](introduction-and-overview/_static/image5.png)

<span data-ttu-id="5060c-196">PayPal potwierdzi Twojego konta, kolejność i informacje o płatności.</span><span class="sxs-lookup"><span data-stu-id="5060c-196">PayPal will confirm your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="5060c-198">Po powrocie z PayPal, możesz sprawdzić i ukończyć zamówienia.</span><span class="sxs-lookup"><span data-stu-id="5060c-198">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys — Przegląd kolejności](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="5060c-200">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5060c-200">Prerequisites</span></span>

<span data-ttu-id="5060c-201">Przed rozpoczęciem upewnij się, że masz następujące oprogramowanie zainstalowane na komputerze:</span><span class="sxs-lookup"><span data-stu-id="5060c-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="5060c-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) lub [programu Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="5060c-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="5060c-203">.NET Framework jest instalowana automatycznie.</span><span class="sxs-lookup"><span data-stu-id="5060c-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="5060c-204">Ten samouczek serii używa programu Microsoft Visual Studio Express 2013 for Web.</span><span class="sxs-lookup"><span data-stu-id="5060c-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="5060c-205">Do ukończenia tego samouczka serii można użyć programu Microsoft Visual Studio Express 2013 for Web albo Microsoft Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="5060c-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5060c-206">Microsoft Visual Studio 2013 i programu Microsoft Visual Studio Express 2013 for Web będzie często określane jako Visual Studio w tej serii samouczka.</span><span class="sxs-lookup"><span data-stu-id="5060c-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="5060c-207">Jeśli masz już zainstalowany wersji programu Visual Studio, proces instalacji zainstaluje Visual Studio 2013 lub Microsoft Visual Studio Express 2013 for Web obok istniejącą wersję.</span><span class="sxs-lookup"><span data-stu-id="5060c-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="5060c-208">Lokacje, które zostały utworzone we wcześniejszych wersjach mogą być otwierane w programie Visual Studio 2013 i nadal otworzyć we wcześniejszych wersjach.</span><span class="sxs-lookup"><span data-stu-id="5060c-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5060c-209">W tym przewodniku przyjęto założenie, że wybrano *projektowanie witryn sieci Web* kolekcję ustawień przy pierwszym uruchomieniu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5060c-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="5060c-210">Aby uzyskać więcej informacji, zobacz [porady: Wybierz ustawienia środowiska sieci Web Development](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="5060c-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="5060c-211">Pobierz aplikację przykładową</span><span class="sxs-lookup"><span data-stu-id="5060c-211">Download the Sample Application</span></span>

<span data-ttu-id="5060c-212">Po zainstalowaniu wymagań wstępnych, można przystąpić do rozpoczęcia tworzenia nowego projektu sieci Web, które są prezentowane w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="5060c-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="5060c-213">Jeśli chcesz **opcjonalnie** Uruchom przykładową aplikację tworzącą tego samouczka serii, można go pobrać z witryny MSDN próbek.</span><span class="sxs-lookup"><span data-stu-id="5060c-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="5060c-214">Ten plik do pobrania zawiera następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="5060c-214">This download contains the following:</span></span>

- <span data-ttu-id="5060c-215">Przykładową aplikację w *WingtipToys* folderu.</span><span class="sxs-lookup"><span data-stu-id="5060c-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="5060c-216">Zasoby używane do tworzenia przykładowej aplikacji w *zasoby WingtipToys* folderu w *WingtipToys* folderu.</span><span class="sxs-lookup"><span data-stu-id="5060c-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="5060c-217">Pobierz plik z witryny MSDN próbek:</span><span class="sxs-lookup"><span data-stu-id="5060c-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="5060c-218">[Wprowadzenie do formularzy sieci Web 4.5 ASP.NET i Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="5060c-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="5060c-219">Pobieranie jest <em>.zip</em> pliku.</span><span class="sxs-lookup"><span data-stu-id="5060c-219">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="5060c-220">Aby wyświetlić ukończone projektu, który tworzy tę serię samouczek, Znajdź i wybierz <em>C#</em>folderu w <em>.zip</em> pliku.</span><span class="sxs-lookup"><span data-stu-id="5060c-220">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="5060c-221">Zapisz <em>C#</em> folderto folderu używać do pracy z projektów Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="5060c-221">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="5060c-222">Domyślnie folderu projektów programu Visual Studio 2013 jest następujący:</span><span class="sxs-lookup"><span data-stu-id="5060c-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="5060c-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual 2013\Projects w Studio</strong></span><span class="sxs-lookup"><span data-stu-id="5060c-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="5060c-224">Zmień nazwę ***C#*** folder ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="5060c-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="5060c-225">Jeśli masz już folder o nazwie *WingtipToys* w folderze projektów tymczasowo zmień nazwę tego folderu istniejących przed zmianą nazwy *C#* folder *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="5060c-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="5060c-226">Aby uruchomić projekt ukończone, otwórz *WingtipToys* folder i kliknij dwukrotnie *WingtipToys.sln* pliku.</span><span class="sxs-lookup"><span data-stu-id="5060c-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="5060c-227">Visual Studio 2013 spowoduje otwarcie projektu.</span><span class="sxs-lookup"><span data-stu-id="5060c-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="5060c-228">Następnie kliknij prawym przyciskiem myszy *Default.aspx* pliku w oknie Eksploratora rozwiązań i kliknij polecenie Wyświetl w przeglądarce w menu kontekstowym.</span><span class="sxs-lookup"><span data-stu-id="5060c-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="5060c-229">Samouczek pomocy technicznej i komentarze</span><span class="sxs-lookup"><span data-stu-id="5060c-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="5060c-230">Użyj sekcji pytania dołączonego [wprowadzenie do formularzy sieci Web programu ASP.NET 4.5 i programu Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) przykładowy (C#) w przypadku jakichkolwiek pytań lub komentarzy.</span><span class="sxs-lookup"><span data-stu-id="5060c-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="5060c-231">Komentarze dotyczące tego samouczka serii są powitalną, a po zaktualizowaniu tego samouczka serii wszelkich starań, zostaną wprowadzone należy wziąć pod uwagę poprawki lub sugestie dotyczące ulepszenia, które znajdują się w samouczku komentarze.</span><span class="sxs-lookup"><span data-stu-id="5060c-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="5060c-232">Po wystąpieniu błędu podczas tworzenia lub witryny sieci Web nie działa poprawnie, komunikaty o błędach mogą spowodować wskazówek złożone źródło problemu lub nie może wyjaśnić, jak to naprawić.</span><span class="sxs-lookup"><span data-stu-id="5060c-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="5060c-233">Pomóc w niektórych typowych scenariuszy problem, można również użyć [fora ASP.NET](https://forums.asp.net/) lub sekcji pytania dołączonego [wprowadzenie do formularzy sieci Web programu ASP.NET 4.5 i programu Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) próbki.</span><span class="sxs-lookup"><span data-stu-id="5060c-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="5060c-234">Jeśli coś nie działa podczas wykonywania kroków samouczków wyświetlony komunikat o błędzie, należy sprawdzić w powyższych lokalizacjach.</span><span class="sxs-lookup"><span data-stu-id="5060c-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5060c-235">Next</span><span class="sxs-lookup"><span data-stu-id="5060c-235">Next</span></span>](create-the-project.md)
