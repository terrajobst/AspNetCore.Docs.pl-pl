---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Wprowadzenie do formularzy sieci Web w wersji 4.7 ASP.NET i programu Visual Studio 2017 | Dokumentacja firmy Microsoft
author: Erikre
description: W tej serii samouczków krok po kroku obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.7 i Microsoft Visual Studio
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: fb41ce72e9454d8d670a0b95234d2bc3f909f0ee
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341556"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a><span data-ttu-id="f4025-103">Wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="f4025-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2017</span></span>
====================

<span data-ttu-id="f4025-104">[Pobierz Wingtip Toys przykładowego projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="f4025-104">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="f4025-105">W tej serii samouczków pokazano, jak skompilować aplikację ASP.NET Web Forms, ASP.NET 4.5 i programu Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="f4025-105">This tutorial series shows you how to build an ASP.NET Web Forms application with ASP.NET 4.5 and Microsoft Visual Studio 2017.</span></span> 

## <a name="introduction"></a><span data-ttu-id="f4025-106">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="f4025-106">Introduction</span></span>

<span data-ttu-id="f4025-107">W tej serii samouczków przeprowadzi Cię przez tworzenie aplikacji formularzy sieci Web ASP.NET przy użyciu programu Visual Studio 2017 i program ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="f4025-107">This tutorial series guides you through creating an ASP.NET Web Forms application using Visual Studio 2017 and ASP.NET 4.5.</span></span> <span data-ttu-id="f4025-108">Utworzymy aplikację o nazwie **Wingtip Toys** — uproszczone storefront witryny sieci web sprzedaż elementów w trybie online.</span><span class="sxs-lookup"><span data-stu-id="f4025-108">You'll create an application named **Wingtip Toys** - a simplified storefront web site selling items online.</span></span> <span data-ttu-id="f4025-109">Podczas serii zostały wyróżnione nowych funkcji ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="f4025-109">During the series, new ASP.NET 4.5 features are highlighted.</span></span>

### <a name="target-audience"></a><span data-ttu-id="f4025-110">Docelowi odbiorcy</span><span class="sxs-lookup"><span data-stu-id="f4025-110">Target audience</span></span>

<span data-ttu-id="f4025-111">Jesteś nowym użytkownikiem formularzy sieci Web platformy ASP.NET dla deweloperów jest przeznaczony dla tej serii samouczków.</span><span class="sxs-lookup"><span data-stu-id="f4025-111">Developers new to ASP.NET Web Forms are the target audience for this tutorial series.</span></span>

<span data-ttu-id="f4025-112">Musisz mieć pewną wiedzę na temat w następujących obszarach:</span><span class="sxs-lookup"><span data-stu-id="f4025-112">You should have some knowledge in the following areas:</span></span>

- <span data-ttu-id="f4025-113">Programowanie zorientowane obiektowo (Obiektowo) i języków</span><span class="sxs-lookup"><span data-stu-id="f4025-113">Object-oriented programming (OOP) and languages</span></span>
- <span data-ttu-id="f4025-114">Tworzenie aplikacji sieci Web (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="f4025-114">Web development (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="f4025-115">Relacyjne bazy danych</span><span class="sxs-lookup"><span data-stu-id="f4025-115">Relational databases</span></span>
- <span data-ttu-id="f4025-116">Architektury N-warstwowej</span><span class="sxs-lookup"><span data-stu-id="f4025-116">N-tier architecture</span></span>

<span data-ttu-id="f4025-117">Aby zapoznać się z tych obszarów, należy wziąć pod uwagę bada następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="f4025-117">To review these areas, consider studying the following content:</span></span>

- [<span data-ttu-id="f4025-118">Wprowadzenie do języka Visual C#</span><span class="sxs-lookup"><span data-stu-id="f4025-118">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="f4025-119">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="f4025-119">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="f4025-120">Relacyjna baza danych</span><span class="sxs-lookup"><span data-stu-id="f4025-120">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="f4025-121">Architektury wielowarstwowej</span><span class="sxs-lookup"><span data-stu-id="f4025-121">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="f4025-122">Funkcje aplikacji</span><span class="sxs-lookup"><span data-stu-id="f4025-122">Application features</span></span>

<span data-ttu-id="f4025-123">Funkcje formularz sieci Web ASP.NET, przedstawione w tej serii obejmują:</span><span class="sxs-lookup"><span data-stu-id="f4025-123">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="f4025-124">Projekt aplikacji sieci Web (nie projekt witryny sieci Web)</span><span class="sxs-lookup"><span data-stu-id="f4025-124">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="f4025-125">Formularze sieci Web</span><span class="sxs-lookup"><span data-stu-id="f4025-125">Web Forms</span></span>
- <span data-ttu-id="f4025-126">Strony wzorcowe, konfiguracji</span><span class="sxs-lookup"><span data-stu-id="f4025-126">Master Pages, Configuration</span></span>
- <span data-ttu-id="f4025-127">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="f4025-127">Bootstrap</span></span>
- <span data-ttu-id="f4025-128">Entity Framework Code najpierw LocalDB</span><span class="sxs-lookup"><span data-stu-id="f4025-128">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="f4025-129">Żądanie weryfikacji</span><span class="sxs-lookup"><span data-stu-id="f4025-129">Request Validation</span></span>
- <span data-ttu-id="f4025-130">Formanty danych silnie typizowane</span><span class="sxs-lookup"><span data-stu-id="f4025-130">Strongly-typed Data Controls</span></span>
- <span data-ttu-id="f4025-131">Wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="f4025-131">Model Binding</span></span>
- <span data-ttu-id="f4025-132">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="f4025-132">Data Annotations</span></span>
- <span data-ttu-id="f4025-133">Dostawców wartości</span><span class="sxs-lookup"><span data-stu-id="f4025-133">Value Providers</span></span>
- <span data-ttu-id="f4025-134">Protokół SSL i uwierzytelniania OAuth</span><span class="sxs-lookup"><span data-stu-id="f4025-134">SSL and OAuth</span></span>
- <span data-ttu-id="f4025-135">ASP.NET Identity, konfiguracji i autoryzacji</span><span class="sxs-lookup"><span data-stu-id="f4025-135">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="f4025-136">Sprawdzania poprawności dyskretnego kodu</span><span class="sxs-lookup"><span data-stu-id="f4025-136">Unobtrusive Validation</span></span>
- <span data-ttu-id="f4025-137">Routing</span><span class="sxs-lookup"><span data-stu-id="f4025-137">Routing</span></span>
- <span data-ttu-id="f4025-138">Obsługa błędów platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f4025-138">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="f4025-139">Scenariusze aplikacji i zadania</span><span class="sxs-lookup"><span data-stu-id="f4025-139">Application scenarios and tasks</span></span>

<span data-ttu-id="f4025-140">Seria samouczków zadania obejmują:</span><span class="sxs-lookup"><span data-stu-id="f4025-140">Tutorial series tasks include:</span></span>

- <span data-ttu-id="f4025-141">Tworzenie, przeglądanie i uruchamianie nowego projektu</span><span class="sxs-lookup"><span data-stu-id="f4025-141">Creating, reviewing, and running a new project</span></span>
- <span data-ttu-id="f4025-142">Tworzenie struktury bazy danych</span><span class="sxs-lookup"><span data-stu-id="f4025-142">Creating a database structure</span></span>
- <span data-ttu-id="f4025-143">Inicjowanie i wstępne wypełnianie bazy danych</span><span class="sxs-lookup"><span data-stu-id="f4025-143">Initializing and seeding a database</span></span>
- <span data-ttu-id="f4025-144">Dostosowywanie interfejsu użytkownika przy użyciu stylów, grafikę i strony wzorcowej</span><span class="sxs-lookup"><span data-stu-id="f4025-144">Customizing the UI with styles, graphics, and a master page</span></span>
- <span data-ttu-id="f4025-145">Dodawanie strony i nawigacja</span><span class="sxs-lookup"><span data-stu-id="f4025-145">Adding pages and navigation</span></span>
- <span data-ttu-id="f4025-146">Wyświetlanie szczegółów menu i danych produktu</span><span class="sxs-lookup"><span data-stu-id="f4025-146">Displaying menu details and product data</span></span>
- <span data-ttu-id="f4025-147">Tworzenie koszyka</span><span class="sxs-lookup"><span data-stu-id="f4025-147">Creating a shopping cart</span></span>
- <span data-ttu-id="f4025-148">Obsługa dodawania SSL i uwierzytelniania OAuth</span><span class="sxs-lookup"><span data-stu-id="f4025-148">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="f4025-149">Dodawanie metody płatności</span><span class="sxs-lookup"><span data-stu-id="f4025-149">Adding a payment method</span></span>
- <span data-ttu-id="f4025-150">W tym rolę administratora i użytkownika do aplikacji</span><span class="sxs-lookup"><span data-stu-id="f4025-150">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="f4025-151">Ograniczanie dostępu do konkretnych stron i folderów</span><span class="sxs-lookup"><span data-stu-id="f4025-151">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="f4025-152">Próba przekazania pliku do aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="f4025-152">Uploading a file to the web application</span></span>
- <span data-ttu-id="f4025-153">Implementowanie walidacji danych wejściowych</span><span class="sxs-lookup"><span data-stu-id="f4025-153">Implementing input validation</span></span>
- <span data-ttu-id="f4025-154">Rejestrowanie tras dla aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="f4025-154">Registering routes for the web application</span></span>
- <span data-ttu-id="f4025-155">Implementowanie obsługi błędów i rejestrowania błędów</span><span class="sxs-lookup"><span data-stu-id="f4025-155">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="f4025-156">Omówienie</span><span class="sxs-lookup"><span data-stu-id="f4025-156">Overview</span></span>

<span data-ttu-id="f4025-157">W tej serii samouczków jest przeznaczony do kogoś zapoznać się z koncepcjami programistycznymi, ale jesteś nowym użytkownikiem formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f4025-157">This tutorial series is intended for someone familiar with programming concepts, but new to ASP.NET Web Forms.</span></span> <span data-ttu-id="f4025-158">Jeśli już znasz z formularzy sieci Web ASP.NET, ta seria nadal może pomóc więcej informacji na temat nowych funkcji ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="f4025-158">If you're already familiar with ASP.NET Web Forms, this series can still help you learn about new ASP.NET 4.5 features.</span></span> <span data-ttu-id="f4025-159">Czytelnicy zaznajomiony z programowania pojęcia i formularzy sieci Web ASP.NET, zobacz dodatkowe samouczki formularzy sieci Web w [wprowadzenie](../../../index.md) sekcji w witrynie sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f4025-159">For readers unfamiliar with programming concepts and ASP.NET Web Forms, see the additional Web Forms tutorials provided in the [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="f4025-160">W tej serii samouczków w programie ASP.NET 4.5 zawiera następujące funkcje:</span><span class="sxs-lookup"><span data-stu-id="f4025-160">The ASP.NET 4.5 provided in this tutorial series includes the following features:</span></span>

- <span data-ttu-id="f4025-161">Prosty interfejs użytkownika do tworzenia projektów oferującą [pomocy technicznej dla wielu platform ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC i interfejs API sieci Web).</span><span class="sxs-lookup"><span data-stu-id="f4025-161">A simple UI for creating projects that offers [support for many ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="f4025-162">[Usługa ładowania początkowego](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), układ, motywy i elastyczne środowisko framework.</span><span class="sxs-lookup"><span data-stu-id="f4025-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout, theming, and responsive design framework.</span></span>
- <span data-ttu-id="f4025-163">[ASP.NET Identity](../../../../identity/index.md), nowego systemu członkostwa programu ASP.NET, która działa tak samo, we wszystkich platform ASP.NET i działa, za pomocą oprogramowania innych niż IIS hostingu w sieci web.</span><span class="sxs-lookup"><span data-stu-id="f4025-163">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- [<span data-ttu-id="f4025-164">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="f4025-164">Entity Framework 6</span></span>](https://msdn.microsoft.com/data/ef.aspx)

  <span data-ttu-id="f4025-165">Aktualizacja programu Entity Framework, dzięki któremu można:</span><span class="sxs-lookup"><span data-stu-id="f4025-165">An update to the Entity Framework enabling you to:</span></span>
  - <span data-ttu-id="f4025-166">Pobieranie i manipulowanie danymi jako silnie typizowanych obiektów</span><span class="sxs-lookup"><span data-stu-id="f4025-166">Retrieve and manipulate data as strongly-typed objects</span></span>
  - <span data-ttu-id="f4025-167">Dostęp do danych w sposób asynchroniczny</span><span class="sxs-lookup"><span data-stu-id="f4025-167">Access data asynchronously</span></span>
  - <span data-ttu-id="f4025-168">Obsługa przejściowe błędy połączenia</span><span class="sxs-lookup"><span data-stu-id="f4025-168">Handle transient connection faults</span></span>
  - <span data-ttu-id="f4025-169">W instrukcjach SQL</span><span class="sxs-lookup"><span data-stu-id="f4025-169">Log SQL statements</span></span>

<span data-ttu-id="f4025-170">Aby uzyskać pełną listę funkcji ASP.NET 4.5, zobacz [ASP.NET and Web Tools dla programu Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="f4025-170">For a complete ASP.NET 4.5 feature list, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="f4025-171">Przykładowej aplikacji Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="f4025-171">The Wingtip Toys sample application</span></span>

<span data-ttu-id="f4025-172">Poniższe zrzuty ekranu są z aplikacji formularzy sieci Web ASP.NET, który zostanie utworzony w tej serii samouczków.</span><span class="sxs-lookup"><span data-stu-id="f4025-172">The following screenshots are from the ASP.NET Web Forms application that you create in this tutorial series.</span></span> <span data-ttu-id="f4025-173">Po uruchomieniu aplikacji w programie Visual Studio pojawia się następującą stronę główną.</span><span class="sxs-lookup"><span data-stu-id="f4025-173">When you run the application in Visual Studio, the following web Home page appears.</span></span>

![Firmy Wingtip Toys — domyślna strona](introduction-and-overview/_static/image1.png)

<span data-ttu-id="f4025-175">Zarejestruj się jako nowy użytkownik lub zaloguj się jako istniejącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f4025-175">You can register as a new user, or sign in as an existing user.</span></span> <span data-ttu-id="f4025-176">Górnym menu nawigacyjnym zawiera linki do kategorie produktów i produktów z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f4025-176">The top navigation has links to product categories and their products from the database.</span></span>

<span data-ttu-id="f4025-177">Jeśli wybierzesz **produktów**, są wyświetlane wszystkie dostępne produkty.</span><span class="sxs-lookup"><span data-stu-id="f4025-177">If you select **Products**, all available products are displayed.</span></span> 

![Firmy Wingtip Toys - produktów](introduction-and-overview/_static/image2.png)

<span data-ttu-id="f4025-179">Jeśli wybierzesz określonego produktu, są wyświetlane szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="f4025-179">If you select a specific product, product details are displayed.</span></span>


![Firmy Wingtip Toys — szczegóły produktu](introduction-and-overview/_static/image3.png)

<span data-ttu-id="f4025-181">Użytkownik może rejestrować i zaloguj się przy użyciu funkcji domyślnego szablonu formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f4025-181">As a user, you can register and sign in with Web Forms template default functionality.</span></span> <span data-ttu-id="f4025-182">W tym samouczku wyjaśniono również, jak zarejestrować się przy użyciu istniejącego konta usługi Gmail.</span><span class="sxs-lookup"><span data-stu-id="f4025-182">This tutorial also explains how to sign in using an existing Gmail account.</span></span> <span data-ttu-id="f4025-183">Ponadto można zalogować się jako administrator, aby dodawać i usuwać produktów z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f4025-183">Additionally, you can sign in as the administrator to add and remove products from the database.</span></span>

![Firmy Wingtip Toys — logowanie](introduction-and-overview/_static/image4.png)

<span data-ttu-id="f4025-185">Po zalogowaniu się jako użytkownik, możesz dodać do koszyka i wyewidencjonowania w systemie PayPal produktów.</span><span class="sxs-lookup"><span data-stu-id="f4025-185">Once you've signed in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="f4025-186">Przykładowa aplikacja jest przeznaczona do pracy w piaskownicy dla deweloperów PayPal.</span><span class="sxs-lookup"><span data-stu-id="f4025-186">The sample application is designed to work in PayPal's developer sandbox.</span></span> <span data-ttu-id="f4025-187">Odbywa się żadna transakcja rzeczywiste pieniądze.</span><span class="sxs-lookup"><span data-stu-id="f4025-187">No actual money transaction takes place.</span></span>

![Firmy Wingtip Toys - koszyk](introduction-and-overview/_static/image5.png)

<span data-ttu-id="f4025-189">PayPal potwierdzenie informacji o koncie, kolejność i płatności.</span><span class="sxs-lookup"><span data-stu-id="f4025-189">PayPal confirms your account, order, and payment information.</span></span>

![Firmy Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="f4025-191">Po powrocie z systemu PayPal, można przejrzeć i ukończyć zamówienie.</span><span class="sxs-lookup"><span data-stu-id="f4025-191">After returning from PayPal, you can review and complete your order.</span></span>

![Firmy Wingtip Toys — Przegląd zamówienia](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="f4025-193">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="f4025-193">Prerequisites</span></span>

<span data-ttu-id="f4025-194">Przed rozpoczęciem upewnij się, że następujące oprogramowanie jest zainstalowane na komputerze:</span><span class="sxs-lookup"><span data-stu-id="f4025-194">Before you start, make sure the following software is installed on your computer:</span></span>

- <span data-ttu-id="f4025-195">[Microsoft Visual Studio 2017 lub Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f4025-195">[Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span></span>

<span data-ttu-id="f4025-196">.NET Framework jest instalowana automatycznie.</span><span class="sxs-lookup"><span data-stu-id="f4025-196">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="f4025-197">W tej serii samouczków używa programu Microsoft Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="f4025-197">This tutorial series uses Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="f4025-198">Można użyć albo lub Microsoft Visual Studio 2017 do ukończenia tej serii samouczków.</span><span class="sxs-lookup"><span data-stu-id="f4025-198">You can use either that or Microsoft Visual Studio 2017 to complete this tutorial series.</span></span>

<span data-ttu-id="f4025-199">Należy pamiętać o następujących dotyczących programu Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="f4025-199">Note the following about Visual Studio:</span></span>

* <span data-ttu-id="f4025-200">Microsoft Visual Studio 2017 i Microsoft Visual Studio Community 2017 są określane jako *programu Visual Studio* w całej tej serii samouczków.</span><span class="sxs-lookup"><span data-stu-id="f4025-200">Microsoft Visual Studio 2017 and Microsoft Visual Studio Community 2017 are referred to as *Visual Studio* throughout this tutorial series.</span></span>

* <span data-ttu-id="f4025-201">Program Visual Studio 2017 jest instalowany obok wszystkie starsze wersje zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="f4025-201">Visual Studio 2017 is installed next to any older versions already installed.</span></span> <span data-ttu-id="f4025-202">Utworzone we wcześniejszych wersjach witryn mogą być otwierane w programie Visual Studio 2017 i Kontynuuj, aby w poprzednich wersjach.</span><span class="sxs-lookup"><span data-stu-id="f4025-202">Sites created in earlier versions can be opened in Visual Studio 2017 and continue to open in previous versions.</span></span>

* <span data-ttu-id="f4025-203">Podczas pierwszego uruchomienia programu Visual Studio, to zakłada, że wybrano *programowania dla sieci Web* ustawienia.</span><span class="sxs-lookup"><span data-stu-id="f4025-203">The first time you started Visual Studio, it is assumed you selected the *Web Development* settings.</span></span> <span data-ttu-id="f4025-204">Aby uzyskać więcej informacji, zobacz [jak: Wybierz ustawienia środowiska programowania dla sieci Web](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="f4025-204">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

<span data-ttu-id="f4025-205">Po zainstalowaniu wymagań wstępnych, możesz rozpocząć tworzenie projektu sieci Web, przedstawione w tej serii samouczków.</span><span class="sxs-lookup"><span data-stu-id="f4025-205">After installing the prerequisites, you're ready to begin creating the Web project presented in this tutorial series.</span></span>

## <a name="download-the-sample-application"></a><span data-ttu-id="f4025-206">Pobieranie przykładowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="f4025-206">Download the sample application</span></span>

 <span data-ttu-id="f4025-207">Dowolnym applicatiion przykładowe ukończone w można pobrać z witryny MSDN przykłady:</span><span class="sxs-lookup"><span data-stu-id="f4025-207">You can download the completed sample applicatiion at anytime from the MSDN Samples site:</span></span>

<span data-ttu-id="f4025-208">[Wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 — firmy Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="f4025-208">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

 <span data-ttu-id="f4025-209">Ten plik do pobrania zawiera następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="f4025-209">This download has the following items:</span></span>

- <span data-ttu-id="f4025-210">Przykładowa aplikacja w *WingtipToys* folderu.</span><span class="sxs-lookup"><span data-stu-id="f4025-210">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="f4025-211">Zasoby używane do tworzenia przykładowej aplikacji w *zasoby WingtipToys* folderu w *WingtipToys* folderu.</span><span class="sxs-lookup"><span data-stu-id="f4025-211">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

<span data-ttu-id="f4025-212">Pliki do pobrania jest *zip* pliku.</span><span class="sxs-lookup"><span data-stu-id="f4025-212">The download is a *.zip* file.</span></span> <span data-ttu-id="f4025-213">Aby wyświetlić ukończone projektu, który tworzy tę serię samouczków, Znajdź i wybierz *C#* folder w pliku zip.</span><span class="sxs-lookup"><span data-stu-id="f4025-213">To see the completed project that this tutorial series creates, find and select the *C#* folder in the .zip file.</span></span> <span data-ttu-id="f4025-214">Zapisz C# folderu do folderu, można użyć do pracy z projektów programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f4025-214">Save the C# folder to the folder you use to work with Visual Studio projects.</span></span> <span data-ttu-id="f4025-215">Domyślnie jest folder projektów programu Visual Studio 2017:</span><span class="sxs-lookup"><span data-stu-id="f4025-215">By default, the Visual Studio 2017 projects folder is:</span></span>

<span data-ttu-id="f4025-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span><span class="sxs-lookup"><span data-stu-id="f4025-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span></span>

<span data-ttu-id="f4025-217">Zmień nazwę ***C#*** folder ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="f4025-217">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="f4025-218">Jeśli masz już folder o nazwie *WingtipToys* w Twoim folderze projektów tymczasowo zmień nazwę tego istniejącego folderu przed zmianą nazwy *C#* folder *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="f4025-218">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>

<span data-ttu-id="f4025-219">Aby uruchomić projekt ukończone, otwórz *WingtipToys* folder i kliknij dwukrotnie plik *WingtipToys.sln* pliku.</span><span class="sxs-lookup"><span data-stu-id="f4025-219">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="f4025-220">Program Visual Studio 2017 zostanie otwarty projekt.</span><span class="sxs-lookup"><span data-stu-id="f4025-220">Visual Studio 2017 opens the project.</span></span> <span data-ttu-id="f4025-221">Następnie kliknij prawym przyciskiem myszy *Default.aspx* w pliku **Eksploratora rozwiązań** i wybierz **Pokaż w przeglądarce**.</span><span class="sxs-lookup"><span data-stu-id="f4025-221">Next, right-click the *Default.aspx* file in **Solution Explorer** and select **View In Browser**.</span></span>

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a><span data-ttu-id="f4025-222">Aby zapoznać się z zawartością kwizu wzorca ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="f4025-222">Take a ASP.NET Web Forms quiz to review content</span></span>

<span data-ttu-id="f4025-223">Po ukończeniu tej serii samouczka kwizu sprawdzić swoją wiedzę i wzmocnienia kluczowych pojęć.</span><span class="sxs-lookup"><span data-stu-id="f4025-223">After completing the tutorial series, take a quiz to test your knowledge and reinforce key concepts.</span></span> <span data-ttu-id="f4025-224">Każde pytanie zawiera wyjaśnienie i linki do dodatkowych wskazówek.</span><span class="sxs-lookup"><span data-stu-id="f4025-224">Each question provides an explanation and links to additional guidance.</span></span>

 * [<span data-ttu-id="f4025-225">Quiz formularzy sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f4025-225">ASP.NET Web Forms Quiz</span></span>](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a><span data-ttu-id="f4025-226">Samouczek pomocy technicznej i komentarze</span><span class="sxs-lookup"><span data-stu-id="f4025-226">Tutorial support and comments</span></span>

<span data-ttu-id="f4025-227">Pytania i komentarze, za pomocą sekcji Q i A dołączonym [wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) strona przykładu.</span><span class="sxs-lookup"><span data-stu-id="f4025-227">For questions and comments, use the Q and A section included on the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span>

<span data-ttu-id="f4025-228">Komentarze dotyczące tej serii samouczków to powitalnej.</span><span class="sxs-lookup"><span data-stu-id="f4025-228">Comments on this tutorial series are welcome.</span></span> <span data-ttu-id="f4025-229">Gdy w tej serii samouczków jest aktualizowany, co wysiłki w celu należy wziąć pod uwagę poprawki lub sugestie dotyczące ulepszenia.</span><span class="sxs-lookup"><span data-stu-id="f4025-229">When this tutorial series is updated, every effort is made to consider corrections or suggestions for improvements.</span></span>

<span data-ttu-id="f4025-230">Jeśli wystąpi błąd, odpowiednie komunikaty o błędach może być mylące, za pomocą żadne dobre wyjaśnienia dotyczące sposobu rozwiązania go.</span><span class="sxs-lookup"><span data-stu-id="f4025-230">If an error occurs, the corresponding error messages could be confusing, with no good explanation on how to fix it.</span></span> <span data-ttu-id="f4025-231">Aby uzyskać pomoc, możesz sprawdzić [fora ASP.NET](https://forums.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="f4025-231">For help, you can check the [ASP.NET forums](https://forums.asp.net/).</span></span> <span data-ttu-id="f4025-232">Innym dobrym źródłem jest sekcji Q i A [wprowadzenie do wzorca ASP.NET 4.5 Web Forms i programu Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) strona przykładu.</span><span class="sxs-lookup"><span data-stu-id="f4025-232">Another good source is the Q and A section in the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="f4025-233">Next</span><span class="sxs-lookup"><span data-stu-id="f4025-233">Next</span></span>](create-the-project.md)
