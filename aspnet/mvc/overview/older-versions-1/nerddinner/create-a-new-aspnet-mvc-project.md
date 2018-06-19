---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Utwórz nowy projekt ASP.NET MVC | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 1 przedstawiono sposób wprowadzone podstawowej struktury NerdDinner w aplikacji.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: d15ca67f0ddd8db6842bc5112996ae2dee433536
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869260"
---
<a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="1ab34-103">Utwórz nowy projekt ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="1ab34-103">Create a New ASP.NET MVC Project</span></span>
====================
<span data-ttu-id="1ab34-104">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1ab34-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="1ab34-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="1ab34-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="1ab34-106">Jest to krok 1 z bezpłatny ["NerdDinner" samouczek aplikacji](introducing-the-nerddinner-tutorial.md) który przeszukiwań przez proces kompilacji mały, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="1ab34-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="1ab34-107">Krok 1 przedstawiono sposób wprowadzone podstawowej struktury NerdDinner w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ab34-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="1ab34-108">Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonanie [pobierania uruchomiona z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczki.</span><span class="sxs-lookup"><span data-stu-id="1ab34-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="1ab34-109">NerdDinner krok 1: Plik -&gt;nowy projekt</span><span class="sxs-lookup"><span data-stu-id="1ab34-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="1ab34-110">Naszej aplikacji NerdDinner rozpocznie się po wybraniu **pliku -&gt;nowy projekt** elementu menu w programie Visual Studio 2008 lub wolnego Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="1ab34-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="1ab34-111">Zostanie wyświetlone okno dialogowe "Nowego projektu".</span><span class="sxs-lookup"><span data-stu-id="1ab34-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="1ab34-112">Aby utworzyć nową aplikację ASP.NET MVC, firma Microsoft, wybierz węzeł "Web" po lewej stronie okna dialogowego i wybierz szablon projektu "Aplikacji sieci Web platformy ASP.NET MVC" po prawej stronie:</span><span class="sxs-lookup"><span data-stu-id="1ab34-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="1ab34-113">*Ważne: Upewnij się, zostały pobrane i zainstalowane ASP.NET MVC — w przeciwnym razie nie będą wyświetlane w oknie dialogowym Nowy projekt. Można użyć V2 z [Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) , jeśli nie został on jeszcze zainstalowany (ASP.NET MVC jest dostępne w ramach "platformy sieci Web -&gt;struktury i środowisk uruchomieniowych" sekcji).*</span><span class="sxs-lookup"><span data-stu-id="1ab34-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="1ab34-114">Firma Microsoft będzie nazwę nowego projektu, któremu zamierzamy utworzyć "NerdDinner", a następnie kliknij przycisk "ok", aby go utworzyć.</span><span class="sxs-lookup"><span data-stu-id="1ab34-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="1ab34-115">Po kliknięciu przycisku "ok" programu Visual Studio zostaną wyświetlone dodatkowe okno z monitem o nas można opcjonalnie utworzyć jednostkowy projekt testowy, jak również nowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ab34-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="1ab34-116">Ten jednostkowy projekt testowy pozwala na tworzenie zautomatyzowanych testów, które pozwalają sprawdzić, funkcje i zachowanie aplikacji (coś omówione zostaną następujące czynności jak zadań do wykonania w dalszej części tego samouczka).</span><span class="sxs-lookup"><span data-stu-id="1ab34-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="1ab34-117">"Test" rozwijaną platform w oknie dialogowym powyżej jest wypełniana wszystkie dostępne szablony ASP.NET MVC jednostki testu projektu zainstalowane na komputerze.</span><span class="sxs-lookup"><span data-stu-id="1ab34-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="1ab34-118">NUnit, MBUnit i XUnit można pobrać wersji.</span><span class="sxs-lookup"><span data-stu-id="1ab34-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="1ab34-119">Obsługiwane jest również wbudowana struktura Visual Studio Test jednostki.</span><span class="sxs-lookup"><span data-stu-id="1ab34-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="1ab34-120">*Uwaga: Platformy testów jednostkowych programu Visual Studio jest dostępne tylko z programu Visual Studio Professional 2008 i nowszych wersjach. Jeśli używasz programu VS 2008 Standard Edition lub Visual Web Developer 2008 Express musisz pobrać i zainstalować rozszerzenia NUnit, MBUnit lub XUnit dla platformy ASP.NET MVC w kolejności dla tego okna dialogowego, który będzie wyświetlany. Okno dialogowe nie będzie wyświetlany, jeśli nie ma żadnych platform testów zainstalowane.*</span><span class="sxs-lookup"><span data-stu-id="1ab34-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="1ab34-121">Firma Microsoft będzie używać domyślnej nazwy "NerdDinner.Tests" dla projektu testowego, tworzonej przez nas i użyj opcji "Programu Visual Studio testu jednostkowego" framework.</span><span class="sxs-lookup"><span data-stu-id="1ab34-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="1ab34-122">Po kliknięciu przycisku przycisk "ok" programu Visual Studio utworzy rozwiązania dla nas z dwa projekty w nim — jeden dla aplikacji sieci web i jeden na potrzeby testów jednostkowych:</span><span class="sxs-lookup"><span data-stu-id="1ab34-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="1ab34-123">Badanie NerdDinner struktury katalogów</span><span class="sxs-lookup"><span data-stu-id="1ab34-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="1ab34-124">Podczas tworzenia nowej aplikacji ASP.NET MVC z programem Visual Studio, automatycznie dodaje liczbę plików i katalogów do projektu:</span><span class="sxs-lookup"><span data-stu-id="1ab34-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="1ab34-125">Projekty składnika ASP.NET MVC domyślnie ma sześć katalogów najwyższego poziomu:</span><span class="sxs-lookup"><span data-stu-id="1ab34-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="1ab34-126">**Katalog**</span><span class="sxs-lookup"><span data-stu-id="1ab34-126">**Directory**</span></span> | <span data-ttu-id="1ab34-127">**Cel**</span><span class="sxs-lookup"><span data-stu-id="1ab34-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="1ab34-128">**/ Kontrolerów**</span><span class="sxs-lookup"><span data-stu-id="1ab34-128">**/Controllers**</span></span> | <span data-ttu-id="1ab34-129">Gdzie umieścić klasy kontrolera, które obsługuje adresu URL żądania</span><span class="sxs-lookup"><span data-stu-id="1ab34-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="1ab34-130">**/ Modeli**</span><span class="sxs-lookup"><span data-stu-id="1ab34-130">**/Models**</span></span> | <span data-ttu-id="1ab34-131">Gdzie umieścić klasy, które reprezentują i manipulowanie danymi</span><span class="sxs-lookup"><span data-stu-id="1ab34-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="1ab34-132">**/ Widoków**</span><span class="sxs-lookup"><span data-stu-id="1ab34-132">**/Views**</span></span> | <span data-ttu-id="1ab34-133">Gdzie umieścić plików szablonów interfejsu użytkownika, które są zobowiązani do renderowania danych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="1ab34-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="1ab34-134">**/ Skryptów**</span><span class="sxs-lookup"><span data-stu-id="1ab34-134">**/Scripts**</span></span> | <span data-ttu-id="1ab34-135">Gdzie umieścić pliki bibliotek JavaScript i skrypty (js)</span><span class="sxs-lookup"><span data-stu-id="1ab34-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="1ab34-136">**/Content**</span><span class="sxs-lookup"><span data-stu-id="1ab34-136">**/Content**</span></span> | <span data-ttu-id="1ab34-137">Gdzie umieścić CSS i pliki obrazów i innej zawartości z systemem innym niż dynamic/z systemem innym niż — JavaScript</span><span class="sxs-lookup"><span data-stu-id="1ab34-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="1ab34-138">**/ Aplikacji\_danych**</span><span class="sxs-lookup"><span data-stu-id="1ab34-138">**/App\_Data**</span></span> | <span data-ttu-id="1ab34-139">Gdzie można przechowywać pliki danych chcesz odczytu/zapisu.</span><span class="sxs-lookup"><span data-stu-id="1ab34-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="1ab34-140">ASP.NET MVC nie wymaga tej struktury.</span><span class="sxs-lookup"><span data-stu-id="1ab34-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="1ab34-141">W rzeczywistości deweloperów pracujących w dużych aplikacji będzie zazwyczaj partycji aplikacji się w wielu projektach, aby była łatwiejsza w zarządzaniu (na przykład: klasy modelu danych często Przejdź w osobnej klasy biblioteki projektu z aplikacji sieci web).</span><span class="sxs-lookup"><span data-stu-id="1ab34-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="1ab34-142">Jednak domyślnej struktury projektu, podaj nieuprzywilejowany domyślnej konwencji katalogu, które pomagają zachować wyczyść zastrzeżenia co do naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ab34-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="1ab34-143">Gdy firma Microsoft rozwiń katalog /Controllers możemy znaleźć, Visual Studio dodane dwie klasy kontrolera — HomeController i elementu AccountController — domyślnie do projektu:</span><span class="sxs-lookup"><span data-stu-id="1ab34-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="1ab34-144">Gdy firma Microsoft rozwiń katalog /Views, znaleźliśmy trzy podkatalogów — /Home, /Account i /Shared —, a także kilka szablon, który domyślnie zawartych w nich plików również zostały dodane do projektu:</span><span class="sxs-lookup"><span data-stu-id="1ab34-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="1ab34-145">Gdy firma Microsoft rozwinąć argumencie / i/skrypty katalogów, możemy znajdziesz obsługę pliku Site.css, który służy do określania stylu HTML wszystkie w witrynie, a także biblioteki JavaScript, w których można włączyć, jQuery i ASP.NET AJAX w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="1ab34-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="1ab34-146">Gdy firma Microsoft rozwiń projekt NerdDinner.Tests znaleźliśmy dwie klasy zawierające testów jednostkowych dla naszej klasy kontrolera:</span><span class="sxs-lookup"><span data-stu-id="1ab34-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="1ab34-147">Te pliki domyślne, dodawane przez program Visual Studio uzyskaliśmy podstawową strukturę dla działającą aplikację - wraz z strony głównej, strony, strony logowania/wylogowania/rejestracji konta i stronę błędu nieobsługiwany (wszystkie przewodowej w górę i jest uruchomiony bez) informacje.</span><span class="sxs-lookup"><span data-stu-id="1ab34-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="1ab34-148">Uruchomienie aplikacji NerdDinner</span><span class="sxs-lookup"><span data-stu-id="1ab34-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="1ab34-149">Można uruchomić projektu, wybierając opcję **debugowania -&gt;Rozpocznij debugowanie** lub **debugowania -&gt;uruchomić bez debugowania** elementów menu:</span><span class="sxs-lookup"><span data-stu-id="1ab34-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="1ab34-150">Spowoduje to Uruchom wbudowane ASP.NET serwera sieci Web-dostarczonego z programem Visual Studio i uruchomić aplikację:</span><span class="sxs-lookup"><span data-stu-id="1ab34-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="1ab34-151">Poniżej znajduje się Strona główna dla naszego nowego projektu (adres URL: "/") po uruchomieniu:</span><span class="sxs-lookup"><span data-stu-id="1ab34-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="1ab34-152">Klikając kartę "Informacje o" Wyświetla o stronie (adres URL: "/ domowej/o"):</span><span class="sxs-lookup"><span data-stu-id="1ab34-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="1ab34-153">Kliknięcie łącza "Logowanie" w prawym górnym rogu powoduje nam do strony logowania (adres URL: "/ Account/logowania")</span><span class="sxs-lookup"><span data-stu-id="1ab34-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="1ab34-154">Nie mamy konto logowania, firma Microsoft może kliknąć łącze rejestru (adres URL: "/ Account/Register") aby go utworzyć:</span><span class="sxs-lookup"><span data-stu-id="1ab34-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="1ab34-155">Kod w celu zaimplementowania domu powyżej i wylogowania / register funkcja została dodana domyślnie, gdy utworzono naszego nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="1ab34-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="1ab34-156">Użyjemy go jako punkt początkowy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="1ab34-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="1ab34-157">Testowanie aplikacji NerdDinner</span><span class="sxs-lookup"><span data-stu-id="1ab34-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="1ab34-158">Jeśli użyto Professional Edition lub nowszej wersji programu Visual Studio 2008, możemy użyć wbudowanych jednostki testowania Obsługa środowiska IDE w programie Visual Studio do projektu testowego:</span><span class="sxs-lookup"><span data-stu-id="1ab34-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="1ab34-159">Wybranie jednej z powyższych opcji Otwórz okienko "Wyników testu" w środowisku IDE i uzyskaliśmy stanu przeszedł/nie przeszedł testów jednostkowych 27 uwzględnione w naszej nowego projektu, które obejmują wbudowanej funkcji:</span><span class="sxs-lookup"><span data-stu-id="1ab34-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="1ab34-160">W dalszej części tego samouczka nasz komunikować się więcej o automatycznych testów i dodać dodatkowych testów, które dotyczą funkcji aplikacji, które wprowadzania.</span><span class="sxs-lookup"><span data-stu-id="1ab34-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="1ab34-161">Następny krok</span><span class="sxs-lookup"><span data-stu-id="1ab34-161">Next Step</span></span>

<span data-ttu-id="1ab34-162">Mamy teraz struktury aplikacji w warstwie podstawowa w miejscu.</span><span class="sxs-lookup"><span data-stu-id="1ab34-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="1ab34-163">Umożliwia teraz [Utwórz bazę danych do przechowywania danych aplikacji](create-a-database.md).</span><span class="sxs-lookup"><span data-stu-id="1ab34-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1ab34-164">[Poprzednie](introducing-the-nerddinner-tutorial.md)
> [dalej](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="1ab34-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
