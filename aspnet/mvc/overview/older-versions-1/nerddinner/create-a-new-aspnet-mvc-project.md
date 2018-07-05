---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Utwórz nowy projekt ASP.NET MVC | Dokumentacja firmy Microsoft
author: microsoft
description: Krok 1 pokazuje, jak umieścić podstawową strukturę aplikacji NerdDinner w miejscu.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 9f5e0b3f82d113fc72ab4002ec8d06ad8444dceb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374279"
---
<a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="ab58e-103">Utwórz nowy projekt ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="ab58e-103">Create a New ASP.NET MVC Project</span></span>
====================
<span data-ttu-id="ab58e-104">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ab58e-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="ab58e-105">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="ab58e-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="ab58e-106">Jest to krok 1 z BEZPŁATNEJ [samouczek aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , przeszukiwania — szczegółowe instrukcje dotyczące tworzenia małych, ale ukończyć, aplikacji sieci web przy użyciu platformy ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="ab58e-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="ab58e-107">Krok 1 pokazuje, jak umieścić podstawową strukturę aplikacji NerdDinner w miejscu.</span><span class="sxs-lookup"><span data-stu-id="ab58e-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="ab58e-108">Jeśli używasz programu ASP.NET MVC 3, zaleca się wykonać [Rozpoczynanie pracy z MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) samouczków.</span><span class="sxs-lookup"><span data-stu-id="ab58e-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="ab58e-109">NerdDinner krok 1: Plik -&gt;nowy projekt</span><span class="sxs-lookup"><span data-stu-id="ab58e-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="ab58e-110">Firma Microsoft rozpocznie się naszej aplikacji NerdDinner, wybierając **plikach&gt;nowy projekt** element menu w programie Visual Studio 2008 lub bezpłatnego programu Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="ab58e-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="ab58e-111">Zostanie wyświetlone okno dialogowe "Nowego projektu".</span><span class="sxs-lookup"><span data-stu-id="ab58e-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="ab58e-112">Aby utworzyć nową aplikację ASP.NET MVC, firma Microsoft, wybierz węzeł "Web" po lewej stronie okna dialogowego i następnie wybierz szablon projektu "Aplikacja sieci Web platformy ASP.NET MVC" po prawej stronie:</span><span class="sxs-lookup"><span data-stu-id="ab58e-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="ab58e-113">*Ważne: Upewnij się, że zostały pobrane i zainstalowane ASP.NET MVC — w przeciwnym razie nie będzie widoczna w oknie dialogowym Nowy projekt. Używasz wersji 2 z [Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) Jeśli nie został on jeszcze zainstalowany (ASP.NET MVC jest dostępna w ramach "platformy sieci Web -&gt;środowisk uruchomieniowych i platform" sekcja).*</span><span class="sxs-lookup"><span data-stu-id="ab58e-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="ab58e-114">Firma Microsoft będzie nazwa nowego projektu, którą zamierzamy utworzyć "NerdDinner", a następnie kliknij przycisk "ok", aby go utworzyć.</span><span class="sxs-lookup"><span data-stu-id="ab58e-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="ab58e-115">Po kliknięciu przycisku "ok" Visual Studio zostanie wyświetlone okno dialogowe dodatkowe, które nam by opcjonalnie utworzyć projekt testu jednostkowego dla nowej aplikacji, a także wyświetla monit o.</span><span class="sxs-lookup"><span data-stu-id="ab58e-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="ab58e-116">Ten projekt testu jednostkowego pozwala tworzyć zautomatyzowane testy weryfikujące funkcji i zachowań, naszej aplikacji (coś omówimy sposób zadań do wykonania w dalszej części tego samouczka).</span><span class="sxs-lookup"><span data-stu-id="ab58e-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="ab58e-117">Lista rozwijana "Test framework" w oknie dialogowym powyżej jest wypełniana wszystkie dostępne szablony ASP.NET MVC jednostki testu projektu zainstalowane na komputerze.</span><span class="sxs-lookup"><span data-stu-id="ab58e-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="ab58e-118">Wersje mogą być pobierane do XUnit, NUnit i MBUnit.</span><span class="sxs-lookup"><span data-stu-id="ab58e-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="ab58e-119">Obsługiwane jest również wbudowane środowiska testów jednostkowych w usłudze Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ab58e-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="ab58e-120">*Uwaga: Środowiska testów jednostkowych programu Visual Studio jest dostępna tylko za pomocą programu Visual Studio Professional 2008 i nowszych wersji. Jeśli używasz programu VS 2008 Standard Edition lub Visual Web Developer 2008 Express musisz pobrać i zainstalować rozszerzenia NUnit, MBUnit lub XUnit dla platformy ASP.NET MVC w kolejności dla tego okna dialogowego do wyświetlenia. Okno dialogowe nie będzie wyświetlany, jeśli nie ma żadnych platform testów zainstalowane.*</span><span class="sxs-lookup"><span data-stu-id="ab58e-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="ab58e-121">Firma Microsoft będzie Użyj domyślnej nazwy "NerdDinner.Tests" dla projektu testowego, tworzonej przez nas, a następnie użyj opcji "Visual Studio testu jednostkowego" framework.</span><span class="sxs-lookup"><span data-stu-id="ab58e-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="ab58e-122">Po kliknięciu przycisku "ok" Visual Studio Utwórz rozwiązanie dla nas przy użyciu dwóch projektów w nim — jeden dla naszej aplikacji internetowej i jeden dla testów jednostkowych:</span><span class="sxs-lookup"><span data-stu-id="ab58e-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="ab58e-123">Badanie struktury katalogów NerdDinner</span><span class="sxs-lookup"><span data-stu-id="ab58e-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="ab58e-124">Po utworzeniu nowej aplikacji platformy ASP.NET MVC z programem Visual Studio automatycznie dodaje wiele plików i katalogów do projektu:</span><span class="sxs-lookup"><span data-stu-id="ab58e-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="ab58e-125">Projekty programu ASP.NET MVC domyślnie mają sześć katalogów najwyższego poziomu:</span><span class="sxs-lookup"><span data-stu-id="ab58e-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="ab58e-126">**Katalog**</span><span class="sxs-lookup"><span data-stu-id="ab58e-126">**Directory**</span></span> | <span data-ttu-id="ab58e-127">**Cel**</span><span class="sxs-lookup"><span data-stu-id="ab58e-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="ab58e-128">**/ Kontrolerów**</span><span class="sxs-lookup"><span data-stu-id="ab58e-128">**/Controllers**</span></span> | <span data-ttu-id="ab58e-129">Gdzie umieścić klasy kontrolera, które obsługuje adres URL żądania</span><span class="sxs-lookup"><span data-stu-id="ab58e-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="ab58e-130">**/ Modeli**</span><span class="sxs-lookup"><span data-stu-id="ab58e-130">**/Models**</span></span> | <span data-ttu-id="ab58e-131">Gdzie umieścić klas, które reprezentują i manipulowanie danymi</span><span class="sxs-lookup"><span data-stu-id="ab58e-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="ab58e-132">**/ Widoków**</span><span class="sxs-lookup"><span data-stu-id="ab58e-132">**/Views**</span></span> | <span data-ttu-id="ab58e-133">Gdzie umieścić pliki szablonów interfejsu użytkownika, które są odpowiedzialne za renderowaniem w danych wyjściowych</span><span class="sxs-lookup"><span data-stu-id="ab58e-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="ab58e-134">**/ Skryptów**</span><span class="sxs-lookup"><span data-stu-id="ab58e-134">**/Scripts**</span></span> | <span data-ttu-id="ab58e-135">Gdzie umieścić pliki biblioteki JavaScript i skrypty (js)</span><span class="sxs-lookup"><span data-stu-id="ab58e-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="ab58e-136">**/Content**</span><span class="sxs-lookup"><span data-stu-id="ab58e-136">**/Content**</span></span> | <span data-ttu-id="ab58e-137">Gdzie umieścić CSS i pliki obrazów i innej zawartości innego niż dynamic/inne niż JavaScript</span><span class="sxs-lookup"><span data-stu-id="ab58e-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="ab58e-138">**/ Aplikacji\_danych**</span><span class="sxs-lookup"><span data-stu-id="ab58e-138">**/App\_Data**</span></span> | <span data-ttu-id="ab58e-139">W przypadku, gdy są przechowywane pliki danych chcesz odczytu/zapisu.</span><span class="sxs-lookup"><span data-stu-id="ab58e-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="ab58e-140">ASP.NET MVC nie wymaga tej struktury.</span><span class="sxs-lookup"><span data-stu-id="ab58e-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="ab58e-141">W rzeczywistości deweloperzy pracujący nad dużych aplikacji będzie zazwyczaj partycji aplikacji się w wielu projektach umożliwiają łatwiejsze w zarządzaniu (na przykład: klasy modelu danych często go w projekcie osobnej klasy biblioteki z aplikacji sieci web).</span><span class="sxs-lookup"><span data-stu-id="ab58e-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="ab58e-142">Jednak domyślnej struktury projektu, zapewniają nieuprzywilejowany domyślnej konwencji katalogu, który możemy użyć, aby zachować czyste starannością aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ab58e-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="ab58e-143">Podczas rozwijania katalogu /Controllers możemy znaleźć, Visual Studio dodaje dwie klasy kontrolera — HomeController i elementu AccountController — domyślnie do projektu:</span><span class="sxs-lookup"><span data-stu-id="ab58e-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="ab58e-144">Gdy firma Microsoft jest rozwiniesz katalogu /Views, znaleźliśmy trzy podkatalogi — /Home, / Account i /Shared —, a także kilka szablon, który domyślnie pliki znajdujące się w ich również zostały dodane do projektu:</span><span class="sxs-lookup"><span data-stu-id="ab58e-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="ab58e-145">Podczas rozwijania argumencie / i katalogi/skrypty, możemy znaleźć obsługę pliku Site.css, który służy do nadawania stylu wszystkich HTML w witrynie, a także biblioteki JavaScript, umożliwiające ASP.NET AJAX i bibliotece jQuery w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="ab58e-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="ab58e-146">Gdy firma Microsoft rozwiń projekt NerdDinner.Tests znaleźliśmy dwóch klas, które zawierają testy jednostkowe dla naszych zajęć kontrolera:</span><span class="sxs-lookup"><span data-stu-id="ab58e-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="ab58e-147">Te pliki domyślne, dodawane przez program Visual Studio Opisz podstawowa struktura dla działającej aplikacji — ze strony głównej o strony, strony logowania/wylogowania/rejestracja konta i na stronie nieobsługiwany błąd (wszystkie przewodowej w górę i działa poza pole).</span><span class="sxs-lookup"><span data-stu-id="ab58e-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="ab58e-148">Uruchomienie aplikacji NerdDinner</span><span class="sxs-lookup"><span data-stu-id="ab58e-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="ab58e-149">Można uruchomić projektu, wybierając opcję **debugowania -&gt;Rozpocznij debugowanie** lub **debugowania -&gt;Rozpocznij bez debugowania** elementy menu:</span><span class="sxs-lookup"><span data-stu-id="ab58e-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="ab58e-150">To spowoduje uruchomienie wbudowanego serwera programu ASP.NET w sieci Web-dostarczanego z programem Visual Studio i uruchomić aplikację:</span><span class="sxs-lookup"><span data-stu-id="ab58e-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="ab58e-151">Poniżej znajduje się Strona główna nasz nowy projekt (adres URL: "/") po uruchomieniu:</span><span class="sxs-lookup"><span data-stu-id="ab58e-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="ab58e-152">Klikając kartę "About" Wyświetla informacje o stronie (adres URL: "/ Home/About"):</span><span class="sxs-lookup"><span data-stu-id="ab58e-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="ab58e-153">Kliknięcie linku "Logowanie" w prawym górnym rogu nam przejście do strony logowania (adres URL: "/ konta/logowania")</span><span class="sxs-lookup"><span data-stu-id="ab58e-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="ab58e-154">Nie mamy konto logowania, możemy kliknąć link Zarejestruj (adres URL: "/ konto/Register") można utworzyć:</span><span class="sxs-lookup"><span data-stu-id="ab58e-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="ab58e-155">Kod implementujący powyżej domu i wylogowania / register funkcja została dodana domyślnie podczas tworzenia nasz nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="ab58e-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="ab58e-156">Użyjemy jej jako punktu wyjścia naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ab58e-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="ab58e-157">Testowanie aplikacji NerdDinner</span><span class="sxs-lookup"><span data-stu-id="ab58e-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="ab58e-158">Firma Microsoft korzystania z wersji Professional lub nowszej wersji programu Visual Studio 2008 możemy użyć wbudowaną jednostkę testowania Obsługa środowiska IDE programu Visual Studio, aby przetestować projekt:</span><span class="sxs-lookup"><span data-stu-id="ab58e-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="ab58e-159">Wybranie jednej z powyższych opcji Otwórz okienko "Wyników testów" w środowisku IDE programu i przekazywania nam ze stanem Powodzenie/Niepowodzenie w ramach testów jednostkowych 27, zawarte w naszym nowym projektem, które obejmują wbudowane funkcje:</span><span class="sxs-lookup"><span data-stu-id="ab58e-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="ab58e-160">W dalszej części tego samouczka będziemy mówić więcej informacji na temat automatycznego testowania i dodać testy jednostkowe dodatkowe, które obejmują funkcje aplikacji, które wdrażamy.</span><span class="sxs-lookup"><span data-stu-id="ab58e-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="ab58e-161">Następny krok</span><span class="sxs-lookup"><span data-stu-id="ab58e-161">Next Step</span></span>

<span data-ttu-id="ab58e-162">Teraz mamy struktury Podstawowa aplikacja w miejscu.</span><span class="sxs-lookup"><span data-stu-id="ab58e-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="ab58e-163">Przejdźmy teraz [utworzyć bazę danych do przechowywania danych aplikacji](create-a-database.md).</span><span class="sxs-lookup"><span data-stu-id="ab58e-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ab58e-164">[Poprzednie](introducing-the-nerddinner-tutorial.md)
> [dalej](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="ab58e-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
