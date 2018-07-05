---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 | Dokumentacja firmy Microsoft
author: Erikre
description: W tej serii samouczków krok po kroku obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio wyrażanie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 55e7a5e8c0c7df9597f8597cba49d33da23e9159
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365409"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="abb21-103">Wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="abb21-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="abb21-104">przez [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="abb21-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="abb21-105">[Pobierz Wingtip Toys przykładowego projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="abb21-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="abb21-106">W tej serii samouczków krok po kroku obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web.</span><span class="sxs-lookup"><span data-stu-id="abb21-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="abb21-107">Quiz formularzy sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="abb21-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="abb21-108">Sprawdź swoją wiedzę i wzmocnić kluczowych pojęć, wykonując Quiz formularzy sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="abb21-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="abb21-109">Ten test został opracowany z myślą z zawartości znajdujących się w tej serii samouczków.</span><span class="sxs-lookup"><span data-stu-id="abb21-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="abb21-110">Pytania quizu zawiera wyjaśnienie wraz z linkami do dodatkowych wskazówek.</span><span class="sxs-lookup"><span data-stu-id="abb21-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="abb21-111">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="abb21-111">Introduction</span></span>

<span data-ttu-id="abb21-112">W tej serii samouczków przeprowadzi Cię przez kroki wymagane do tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu programu Visual Studio Express 2013 for Web i program ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="abb21-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="abb21-113">Nosi nazwę aplikacji, należy utworzyć **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="abb21-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="abb21-114">Jest to uproszczony przykład witryny sieci web frontonu magazynu, która sprzedaje elementów w trybie online.</span><span class="sxs-lookup"><span data-stu-id="abb21-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="abb21-115">W tej serii samouczków wyróżnia nowych funkcji dostępnych w programie ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="abb21-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="abb21-116">Komentarze są powitalnej, a my sprawiamy, żeby wszelkich starań, aby zaktualizować tę serię samouczków, w oparciu o Twoje sugestie.</span><span class="sxs-lookup"><span data-stu-id="abb21-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="abb21-117">Zakończono pobieranie projektu</span><span class="sxs-lookup"><span data-stu-id="abb21-117">Download completed project</span></span>

<span data-ttu-id="abb21-118">Możesz pobrać projekt C#, który zawiera samouczek ukończone.</span><span class="sxs-lookup"><span data-stu-id="abb21-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="abb21-119">[Wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 — firmy Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="abb21-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="abb21-120">Przejrzyj zawartość, wykonując powiązane quiz wzorca ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="abb21-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="abb21-121">Po ukończeniu tego samouczka sprawdzić swoją wiedzę i wzmocnić kluczowych pojęć, wykonując [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="abb21-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="abb21-122">Ten test został opracowany z myślą z zawartości znajdujących się w tej serii samouczków.</span><span class="sxs-lookup"><span data-stu-id="abb21-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="abb21-123">Pytania quizu zawiera wyjaśnienie wraz z linkami do dodatkowych wskazówek.</span><span class="sxs-lookup"><span data-stu-id="abb21-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="abb21-124">Quiz formularzy sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="abb21-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="abb21-125">Odbiorcy</span><span class="sxs-lookup"><span data-stu-id="abb21-125">Audience</span></span>

<span data-ttu-id="abb21-126">Odbiorców tej serii samouczków jest doświadczonych deweloperów, którzy dopiero formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="abb21-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="abb21-127">Zainteresowane w tej serii samouczków Deweloper powinien mieć następujące umiejętności:</span><span class="sxs-lookup"><span data-stu-id="abb21-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="abb21-128">Powszechnie znane z obiektem obiektowo języka programowania (Obiektowo)</span><span class="sxs-lookup"><span data-stu-id="abb21-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="abb21-129">Znasz koncepcji programowania w sieci Web (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="abb21-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="abb21-130">Znasz koncepcji relacyjnych baz danych</span><span class="sxs-lookup"><span data-stu-id="abb21-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="abb21-131">Znasz koncepcji architektury n warstwowej</span><span class="sxs-lookup"><span data-stu-id="abb21-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="abb21-132">Jeśli jesteś zainteresowani zapoznaniem obszarów wymienionych powyżej, należy wziąć pod uwagę przejrzeniu następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="abb21-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="abb21-133">Wprowadzenie do języka Visual C#</span><span class="sxs-lookup"><span data-stu-id="abb21-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="abb21-134">[Opracowywanie sieci Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP i JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="abb21-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="abb21-135">Relacyjna baza danych</span><span class="sxs-lookup"><span data-stu-id="abb21-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="abb21-136">Architektury wielowarstwowej</span><span class="sxs-lookup"><span data-stu-id="abb21-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="abb21-137">Funkcje aplikacji</span><span class="sxs-lookup"><span data-stu-id="abb21-137">Application Features</span></span>

<span data-ttu-id="abb21-138">Funkcje formularz sieci Web ASP.NET, przedstawione w tej serii obejmują:</span><span class="sxs-lookup"><span data-stu-id="abb21-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="abb21-139">Projekt aplikacji sieci Web (nie projekt witryny sieci Web)</span><span class="sxs-lookup"><span data-stu-id="abb21-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="abb21-140">Formularze sieci Web</span><span class="sxs-lookup"><span data-stu-id="abb21-140">Web Forms</span></span>
- <span data-ttu-id="abb21-141">Strony wzorcowe, konfiguracji</span><span class="sxs-lookup"><span data-stu-id="abb21-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="abb21-142">Usługa ładowania początkowego</span><span class="sxs-lookup"><span data-stu-id="abb21-142">Bootstrap</span></span>
- <span data-ttu-id="abb21-143">Entity Framework Code najpierw LocalDB</span><span class="sxs-lookup"><span data-stu-id="abb21-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="abb21-144">Żądanie weryfikacji</span><span class="sxs-lookup"><span data-stu-id="abb21-144">Request Validation</span></span>
- <span data-ttu-id="abb21-145">Silnie Typizowane kontrolki danych powiązanie adnotacje danych modelu i dostawców wartości</span><span class="sxs-lookup"><span data-stu-id="abb21-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="abb21-146">Protokół SSL i uwierzytelniania OAuth</span><span class="sxs-lookup"><span data-stu-id="abb21-146">SSL and OAuth</span></span>
- <span data-ttu-id="abb21-147">ASP.NET Identity, konfiguracji i autoryzacji</span><span class="sxs-lookup"><span data-stu-id="abb21-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="abb21-148">Sprawdzania poprawności dyskretnego kodu</span><span class="sxs-lookup"><span data-stu-id="abb21-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="abb21-149">Routing</span><span class="sxs-lookup"><span data-stu-id="abb21-149">Routing</span></span>
- <span data-ttu-id="abb21-150">Obsługa błędów platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="abb21-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="abb21-151">Scenariusze aplikacji i zadania</span><span class="sxs-lookup"><span data-stu-id="abb21-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="abb21-152">Zadania przedstawione w tej serii obejmują:</span><span class="sxs-lookup"><span data-stu-id="abb21-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="abb21-153">Tworzenie, przeglądanie i uruchamianie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="abb21-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="abb21-154">Tworzenie struktury bazy danych</span><span class="sxs-lookup"><span data-stu-id="abb21-154">Creating the database structure</span></span>
- <span data-ttu-id="abb21-155">Inicjowanie i wstępne wypełnianie bazy danych</span><span class="sxs-lookup"><span data-stu-id="abb21-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="abb21-156">Dostosowywanie interfejsu użytkownika przy użyciu stylów, grafiki i strony wzorcowej</span><span class="sxs-lookup"><span data-stu-id="abb21-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="abb21-157">Dodawanie strony i nawigacja</span><span class="sxs-lookup"><span data-stu-id="abb21-157">Adding pages and navigation</span></span>
- <span data-ttu-id="abb21-158">Wyświetlanie szczegółów menu i danych produktu</span><span class="sxs-lookup"><span data-stu-id="abb21-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="abb21-159">Tworzenie koszyka</span><span class="sxs-lookup"><span data-stu-id="abb21-159">Creating a shopping cart</span></span>
- <span data-ttu-id="abb21-160">Obsługa dodawania SSL i uwierzytelniania OAuth</span><span class="sxs-lookup"><span data-stu-id="abb21-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="abb21-161">Dodawanie metody płatności</span><span class="sxs-lookup"><span data-stu-id="abb21-161">Adding a payment method</span></span>
- <span data-ttu-id="abb21-162">W tym rolę administratora i użytkownika do aplikacji</span><span class="sxs-lookup"><span data-stu-id="abb21-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="abb21-163">Ograniczanie dostępu do konkretnych stron i folderów</span><span class="sxs-lookup"><span data-stu-id="abb21-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="abb21-164">Próba przekazania pliku do aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="abb21-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="abb21-165">Implementowanie walidacji danych wejściowych</span><span class="sxs-lookup"><span data-stu-id="abb21-165">Implementing input validation</span></span>
- <span data-ttu-id="abb21-166">Rejestrowanie tras dla aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="abb21-166">Registering routes for the web application</span></span>
- <span data-ttu-id="abb21-167">Implementowanie obsługi błędów i rejestrowania błędów</span><span class="sxs-lookup"><span data-stu-id="abb21-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="abb21-168">Omówienie</span><span class="sxs-lookup"><span data-stu-id="abb21-168">Overview</span></span>

<span data-ttu-id="abb21-169">Jeśli dopiero zaczynasz korzystać z ASP.NET Web Forms ale znajomość pojęcia związane z programowaniem, masz prawo samouczka.</span><span class="sxs-lookup"><span data-stu-id="abb21-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="abb21-170">Jeśli masz już zapoznać się z wzorca ASP.NET Web Forms, mogą korzystać z tej serii samouczków, nowe funkcje dostępne w programie ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="abb21-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="abb21-171">Jeśli jesteś zaznajomiony z programowania pojęcia i formularzy sieci Web ASP.NET, zobacz dodatkowe samouczki w formularzach sieci Web [wprowadzenie](../../../index.md) sekcji w witrynie sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="abb21-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="abb21-172">Konkretne **najnowsze** ASP.NET 4.5 funkcje wprowadzone w formularzach sieci Web, ta seria samouczków obejmują następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="abb21-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="abb21-173">Prosty interfejs użytkownika do tworzenia projektów tę ofertę [Obsługa wielu platform ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC i interfejs API sieci Web).</span><span class="sxs-lookup"><span data-stu-id="abb21-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="abb21-174">[Usługa ładowania początkowego](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), układ i motywów strukturę, która oferuje elastyczny możliwości projektowania i motywów.</span><span class="sxs-lookup"><span data-stu-id="abb21-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="abb21-175">[ASP.NET Identity](../../../../identity/index.md), nowego systemu członkostwa programu ASP.NET, która działa tak samo, we wszystkich platform ASP.NET i działa, za pomocą oprogramowania innych niż IIS hostingu w sieci web.</span><span class="sxs-lookup"><span data-stu-id="abb21-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="abb21-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), aktualizację programu Entity Framework, która umożliwia pobieranie i manipulowanie danymi jako silnie typizowanych obiektów, dostęp do danych asynchronicznie, obsługi przejściowe błędy połączenia i zaloguj się instrukcji języka SQL.</span><span class="sxs-lookup"><span data-stu-id="abb21-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="abb21-177">Aby uzyskać pełną listę funkcji ASP.NET 4.5, zobacz [ASP.NET and Web Tools dla programu Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="abb21-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="abb21-178">Firmy Wingtip Toys przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="abb21-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="abb21-179">Poniższe zrzuty ekranu zapewniają szybki przegląd aplikacji formularzy sieci Web ASP.NET, która zostanie utworzona w tej serii samouczków.</span><span class="sxs-lookup"><span data-stu-id="abb21-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="abb21-180">Po uruchomieniu aplikacji z programu Visual Studio Express 2013 for Web zobaczysz poniższą stronę główną sieci web.</span><span class="sxs-lookup"><span data-stu-id="abb21-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Firmy Wingtip Toys — domyślna strona](introduction-and-overview/_static/image1.png)

<span data-ttu-id="abb21-182">Zarejestruj się jako nowy użytkownik lub zaloguj się jako istniejącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="abb21-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="abb21-183">Nawigacji podano u góry dla każdej kategorii produktów, pobierając dostępne produkty z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="abb21-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="abb21-184">Wybierając łącze produktów, można wyświetlić listę wszystkich dostępnych produktów.</span><span class="sxs-lookup"><span data-stu-id="abb21-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Firmy Wingtip Toys - produktów](introduction-and-overview/_static/image2.png)

<span data-ttu-id="abb21-186">Można również wyświetlić szczegółowe informacje indywidualnych produktów, wybierając jedną z wymienionych produktów.</span><span class="sxs-lookup"><span data-stu-id="abb21-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Firmy Wingtip Toys — szczegóły produktu](introduction-and-overview/_static/image3.png)

<span data-ttu-id="abb21-188">Użytkownik może rejestrować i zaloguj się przy użyciu funkcji domyślny szablon formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="abb21-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="abb21-189">W tym samouczku wyjaśniono również, jak zalogować się przy użyciu istniejącego konta Gmail.</span><span class="sxs-lookup"><span data-stu-id="abb21-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="abb21-190">Ponadto możesz zalogować się jako administrator, aby dodawać i usuwać produktów z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="abb21-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Firmy Wingtip Toys — logowanie](introduction-and-overview/_static/image4.png)

<span data-ttu-id="abb21-192">Po zalogowaniu się jako użytkownik, możesz dodać produkty do koszyka i finalizacja zakupu z systemu PayPal.</span><span class="sxs-lookup"><span data-stu-id="abb21-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="abb21-193">Należy pamiętać, że ta Przykładowa aplikacja jest przeznaczona do funkcją piaskownicy dla deweloperów PayPal.</span><span class="sxs-lookup"><span data-stu-id="abb21-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="abb21-194">Żadna transakcja rzeczywiste pieniądze zostaną zrealizowane.</span><span class="sxs-lookup"><span data-stu-id="abb21-194">No actual money transaction will take place.</span></span>

![Firmy Wingtip Toys - koszyk](introduction-and-overview/_static/image5.png)

<span data-ttu-id="abb21-196">PayPal potwierdzi, Twoje konto, kolejność i informacje o płatności.</span><span class="sxs-lookup"><span data-stu-id="abb21-196">PayPal will confirm your account, order, and payment information.</span></span>

![Firmy Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="abb21-198">Po powrocie z systemu PayPal, można przejrzeć i ukończyć zamówienie.</span><span class="sxs-lookup"><span data-stu-id="abb21-198">After returning from PayPal, you can review and complete your order.</span></span>

![Firmy Wingtip Toys — Przegląd zamówienia](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="abb21-200">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="abb21-200">Prerequisites</span></span>

<span data-ttu-id="abb21-201">Przed rozpoczęciem upewnij się, że masz zainstalowane na komputerze następujące oprogramowanie:</span><span class="sxs-lookup"><span data-stu-id="abb21-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="abb21-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) lub [programu Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="abb21-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="abb21-203">.NET Framework jest instalowana automatycznie.</span><span class="sxs-lookup"><span data-stu-id="abb21-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="abb21-204">W tej serii samouczków używa programu Microsoft Visual Studio Express 2013 for Web.</span><span class="sxs-lookup"><span data-stu-id="abb21-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="abb21-205">Aby ukończyć tę serię samouczków, można użyć programu Microsoft Visual Studio Express 2013 for Web albo programu Microsoft Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="abb21-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="abb21-206">Microsoft Visual Studio 2013 i Microsoft Visual Studio Express 2013 for Web będzie często określane jako programu Visual Studio w całej tej serii samouczków.</span><span class="sxs-lookup"><span data-stu-id="abb21-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="abb21-207">Jeśli masz już zainstalowaną wersję programu Visual Studio, proces instalacji instaluje Visual Studio 2013 lub programu Microsoft Visual Studio Express 2013 for Web obok istniejącą wersję.</span><span class="sxs-lookup"><span data-stu-id="abb21-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="abb21-208">Lokacje, które zostały utworzone w starszych wersjach mogą być otwierane w programie Visual Studio 2013 i Kontynuuj, aby w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="abb21-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="abb21-209">W tym przewodniku przyjęto założenie, że wybrana *programowania dla sieci Web* zbiór ustawień podczas pierwszego uruchomienia programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="abb21-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="abb21-210">Aby uzyskać więcej informacji, zobacz [porady: Wybieranie ustawienia środowiska programowania sieci Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="abb21-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="abb21-211">Pobieranie przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="abb21-211">Download the Sample Application</span></span>

<span data-ttu-id="abb21-212">Po zainstalowaniu wymagań wstępnych, można przystąpić do rozpoczęcia tworzenia nowego projektu sieci Web, które są prezentowane w tej serii samouczków.</span><span class="sxs-lookup"><span data-stu-id="abb21-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="abb21-213">Jeśli chcesz **opcjonalnie** uruchamianie przykładowej aplikacji, który tworzy tę serię samouczków, możesz ją pobrać z witryny MSDN przykładów.</span><span class="sxs-lookup"><span data-stu-id="abb21-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="abb21-214">Ten plik do pobrania zawiera następujące informacje:</span><span class="sxs-lookup"><span data-stu-id="abb21-214">This download contains the following:</span></span>

- <span data-ttu-id="abb21-215">Przykładowa aplikacja w *WingtipToys* folderu.</span><span class="sxs-lookup"><span data-stu-id="abb21-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="abb21-216">Zasoby używane do tworzenia przykładowej aplikacji w *zasoby WingtipToys* folderu w *WingtipToys* folderu.</span><span class="sxs-lookup"><span data-stu-id="abb21-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="abb21-217">Pobierz plik z witryny MSDN próbek:</span><span class="sxs-lookup"><span data-stu-id="abb21-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="abb21-218">[Wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 — firmy Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="abb21-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="abb21-219">Pliki do pobrania jest <em>zip</em> pliku.</span><span class="sxs-lookup"><span data-stu-id="abb21-219">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="abb21-220">Aby wyświetlić ukończone projektu, który tworzy tę serię samouczków, Znajdź i wybierz <em>C#</em>folderu w <em>zip</em> pliku.</span><span class="sxs-lookup"><span data-stu-id="abb21-220">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="abb21-221">Zapisz <em>C#</em> folderto folderu do pracy z projektami Visual Studio 2013 można użyć.</span><span class="sxs-lookup"><span data-stu-id="abb21-221">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="abb21-222">Domyślnie folder projektów programu Visual Studio 2013 jest następująca:</span><span class="sxs-lookup"><span data-stu-id="abb21-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="abb21-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span><span class="sxs-lookup"><span data-stu-id="abb21-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="abb21-224">Zmień nazwę ***C#*** folder ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="abb21-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="abb21-225">Jeśli masz już folder o nazwie *WingtipToys* w Twoim folderze projektów tymczasowo zmień nazwę tego istniejącego folderu przed zmianą nazwy *C#* folder *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="abb21-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="abb21-226">Aby uruchomić projekt ukończone, otwórz *WingtipToys* folder i kliknij dwukrotnie plik *WingtipToys.sln* pliku.</span><span class="sxs-lookup"><span data-stu-id="abb21-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="abb21-227">Visual Studio 2013, zostanie otwarty projekt.</span><span class="sxs-lookup"><span data-stu-id="abb21-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="abb21-228">Następnie kliknij prawym przyciskiem myszy *Default.aspx* pliku w oknie Eksploratora rozwiązań, a następnie kliknij pozycję Pokaż w przeglądarce z menu podręcznego.</span><span class="sxs-lookup"><span data-stu-id="abb21-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="abb21-229">Samouczek pomocy technicznej i komentarze</span><span class="sxs-lookup"><span data-stu-id="abb21-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="abb21-230">Sekcja Q i A dołączone [wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) przykładowy (C#) w przypadku jakichkolwiek pytań lub komentarzy.</span><span class="sxs-lookup"><span data-stu-id="abb21-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="abb21-231">Komentarze dotyczące tej serii samouczków są powitalnej, a po zaktualizowaniu tej serii samouczków wszelkich starań, będzie nawiązywane w przypadku uwzględnienia poprawki konta lub sugestie dotyczące ulepszeń, które znajdują się w samouczku komentarzy.</span><span class="sxs-lookup"><span data-stu-id="abb21-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="abb21-232">Po wystąpieniu błędu podczas tworzenia lub witryny sieci Web nie działa poprawnie, komunikaty o błędach mogą spowodować wskazówek złożone źródło problemu, lub nie może wyjaśnić, jak go naprawić.</span><span class="sxs-lookup"><span data-stu-id="abb21-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="abb21-233">Do udzielenia odpowiedzi na kilka typowych scenariuszy, problem, można również użyć [fora ASP.NET](https://forums.asp.net/) lub sekcji Q i A dołączone [wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) próbki.</span><span class="sxs-lookup"><span data-stu-id="abb21-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="abb21-234">Jeśli otrzymasz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczków, pamiętaj, że Sprawdzanie lokalizacji powyżej.</span><span class="sxs-lookup"><span data-stu-id="abb21-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="abb21-235">Next</span><span class="sxs-lookup"><span data-stu-id="abb21-235">Next</span></span>](create-the-project.md)
