---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Dodawanie walidacji do modelu | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utwórz prostą aplikację sieci web odczytuje i zapisuje z bazy danych.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 78dd6bdd81fcb51a3a21a8f1ee12b4b2bfc37db5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871951"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="fc3ec-104">Dodawanie walidacji do modelu</span><span class="sxs-lookup"><span data-stu-id="fc3ec-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="fc3ec-105">przez [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="fc3ec-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="fc3ec-106">Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="fc3ec-107">Utworzysz prostą aplikację sieci web odczytuje i zapisuje z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="fc3ec-108">Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczki i przykłady.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="fc3ec-109">W tej sekcji zamierzamy Obsługa niezbędne do obsługi sprawdzania poprawności danych wejściowych w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="fc3ec-110">Firma Microsoft będzie Sprawdź, czy zawartość bazy danych zawsze jest poprawna i udostępnić komunikaty przydatne dla użytkowników końcowych, jeśli spróbuj i wprowadź dane Movie, która jest nieprawidłowa.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="fc3ec-111">Rozpocznie się przez dodanie do klasy filmu niewielką ilością logiki sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="fc3ec-112">Kliknij prawym przyciskiem folder modelu i wybierz opcję Dodaj klasę.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="fc3ec-113">Nazwa klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-113">Name your class Movie.</span></span>

<span data-ttu-id="fc3ec-114">Gdy modelu jednostki filmu utworzony wcześniej, IDE utworzone klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="fc3ec-115">W rzeczywistości część klasy filmu może być w jednym pliku, a część w innym.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="fc3ec-116">Jest to klasy częściowej.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-116">This is called a Partial Class.</span></span> <span data-ttu-id="fc3ec-117">Chcemy rozszerzenie klasy Movie z innego pliku.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="fc3ec-118">Utworzymy klasy częściowe filmu się "Klasa buddy" z niektóre atrybuty, które zapewni wskazówek weryfikacji do systemu.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="fc3ec-119">Firma Microsoft będzie Oznacz tytuł i cen wymagane i domagać również, że ceny można w pewnym zakresie.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="fc3ec-120">Kliknij prawym przyciskiem myszy folder modeli i wybierz opcję Dodaj klasę.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="fc3ec-121">Nazwa klasy film, a następnie kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="fc3ec-122">Oto, co naszych częściowe filmu klasy prawdopodobnie.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="fc3ec-123">Ponownie uruchom aplikację i spróbuj wprowadzić filmu cena ponad 100.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="fc3ec-124">Zostanie wyświetlony błąd, po przesłaniu formularza.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="fc3ec-125">Błąd zostanie przechwycony po stronie serwera i po opublikowania formularza.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="fc3ec-126">Zwróć uwagę, jak inteligentne, wyświetlony komunikat o błędzie i Obsługa wartości firmie Microsoft w elementach pole tekstowe zostały wbudowane pomocników HTML ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="fc3ec-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="fc3ec-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fc3ec-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="fc3ec-128">To rozwiązanie idealne, ale byłoby nieuprzywilejowany możemy podać użytkownika po stronie klienta, natychmiast, zanim pobiera związane z serwera.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="fc3ec-129">Umożliwia włączenie niektórych weryfikacji po stronie klienta z użyciem języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="fc3ec-130">Dodawanie walidacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="fc3ec-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="fc3ec-131">Ponieważ klasy Nasze filmu już niektóre atrybuty weryfikacji, po prostu musimy dodać kilka plików JavaScript do naszych Create.aspx wyświetlanie szablonu i dodanie wiersza kodu w celu włączenia weryfikacji po stronie klienta odbywa się.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="fc3ec-132">Z poziomu VWD przejdź do folderu naszych widoków/film i otwarcie Create.aspx.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="fc3ec-133">Otwórz folder skryptów w Eksploratorze rozwiązań i przeciągnij następujące trzy skrypty w &lt;head&gt; tagu.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="fc3ec-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="fc3ec-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="fc3ec-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="fc3ec-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="fc3ec-136">Chcesz, aby te pliki skryptów pojawią się w tej kolejności.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="fc3ec-137">Ponadto Dodaj pojedynczy wiersz powyżej Html.BeginForm:</span><span class="sxs-lookup"><span data-stu-id="fc3ec-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="fc3ec-138">Oto kod wyświetlany w środowisku IDE.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="fc3ec-139">[![Filmów - Microsoft Visual Web Developer Express 2010 (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="fc3ec-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="fc3ec-140">Uruchom aplikację ponownie odwiedź /Movies/Create i kliknij przycisk Utwórz bez wprowadzania żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="fc3ec-141">Komunikaty o błędach pojawiają się natychmiast bez flash, że powiązane z wysyłanie danych strony wszystkie sposób z powrotem do serwera.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="fc3ec-142">Jest tak, ponieważ ASP.NET MVC jest teraz Walidacja danych wejściowych, zarówno klienta (przy użyciu języka JavaScript) i na serwerze.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="fc3ec-143">[![Tworzenie - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="fc3ec-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="fc3ec-144">Zależy to dobry!</span><span class="sxs-lookup"><span data-stu-id="fc3ec-144">This is looking good!</span></span> <span data-ttu-id="fc3ec-145">Teraz Dodajmy jedną dodatkową kolumnę w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="fc3ec-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fc3ec-146">[Poprzednie](getting-started-with-mvc-part6.md)
> [dalej](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="fc3ec-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
