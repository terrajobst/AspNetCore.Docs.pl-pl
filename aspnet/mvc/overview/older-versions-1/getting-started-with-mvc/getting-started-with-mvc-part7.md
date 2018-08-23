---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Dodawanie weryfikacji do modelu | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utwórz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 4c7867587ba0610f0f1c23d9a0b9fbdc4040de7c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757360"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="3870d-104">Dodawanie weryfikacji do modelu</span><span class="sxs-lookup"><span data-stu-id="3870d-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="3870d-105">przez [Scotta Hanselmana](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="3870d-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="3870d-106">Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3870d-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="3870d-107">Utworzysz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3870d-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="3870d-108">Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczków i przykładów.</span><span class="sxs-lookup"><span data-stu-id="3870d-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="3870d-109">W tej sekcji użyjemy Obsługa niezbędne do obsługi sprawdzania poprawności danych wejściowych w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3870d-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="3870d-110">Firma Microsoft będzie zagwarantować, że naszej zawartości bazy danych zawsze jest poprawna i podaj komunikaty o błędach pomocne dla użytkowników końcowych, gdy próbują i wprowadź dane filmów, która jest nieprawidłowa.</span><span class="sxs-lookup"><span data-stu-id="3870d-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="3870d-111">Rozpocznie się, dodając niewielką logikę walidacji do klas filmu.</span><span class="sxs-lookup"><span data-stu-id="3870d-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="3870d-112">Kliknij prawym przyciskiem folder modelu, a następnie wybierz pozycję Dodaj klasę.</span><span class="sxs-lookup"><span data-stu-id="3870d-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="3870d-113">Nazwa klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="3870d-113">Name your class Movie.</span></span>

<span data-ttu-id="3870d-114">Podczas tworzenia modelu Entity Movie wcześniej, IDE utworzone klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="3870d-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="3870d-115">W rzeczywistości częścią klasy filmu może być w jednym pliku, a część w innym.</span><span class="sxs-lookup"><span data-stu-id="3870d-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="3870d-116">Jest to nazywane klasy częściowej.</span><span class="sxs-lookup"><span data-stu-id="3870d-116">This is called a Partial Class.</span></span> <span data-ttu-id="3870d-117">Zamierzamy rozszerzyć klasę film z innego pliku.</span><span class="sxs-lookup"><span data-stu-id="3870d-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="3870d-118">Utworzymy klasy częściowej filmu odwołujący się do "buddy class" z niektórych atrybutów, które umożliwia uzyskanie wskazówek dotyczących sprawdzania poprawności do systemu.</span><span class="sxs-lookup"><span data-stu-id="3870d-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="3870d-119">Firma Microsoft będzie należy oznaczyć tytuł i ceny, ponieważ wymagana, a następnie nalegać również, że cena można w pewnym zakresie.</span><span class="sxs-lookup"><span data-stu-id="3870d-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="3870d-120">Kliknij prawym przyciskiem myszy folderu modeli, a następnie wybierz pozycję Dodaj klasę.</span><span class="sxs-lookup"><span data-stu-id="3870d-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="3870d-121">Nazwa klasy filmu, a następnie kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="3870d-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="3870d-122">Oto, co nasz częściowe filmu klasy wygląda.</span><span class="sxs-lookup"><span data-stu-id="3870d-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="3870d-123">Uruchom ponownie aplikację i spróbuj wprowadzić film z ceną ponad 100.</span><span class="sxs-lookup"><span data-stu-id="3870d-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="3870d-124">Zostanie wyświetlony błąd, po przesłaniu formularza.</span><span class="sxs-lookup"><span data-stu-id="3870d-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="3870d-125">Błąd zostanie przechwycony po stronie serwera i występuje po opublikowaniu formularza.</span><span class="sxs-lookup"><span data-stu-id="3870d-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="3870d-126">Zwróć uwagę, jak pomocników HTML wbudowanych w platformy ASP.NET MVC zostały inteligentnego wyświetlić komunikat o błędzie i Obsługa wartości dla nas w elementach pole tekstowe:</span><span class="sxs-lookup"><span data-stu-id="3870d-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="3870d-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3870d-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="3870d-128">Działa to świetnie, ale dobrze byłoby, jeśli można o użytkownika po stronie klienta, natychmiast, zanim serwer pobiera związane.</span><span class="sxs-lookup"><span data-stu-id="3870d-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="3870d-129">Umożliwia włączenie niektórych weryfikacji po stronie klienta za pomocą języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3870d-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="3870d-130">Dodawanie walidacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="3870d-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="3870d-131">Ponieważ klasa nasz film zawiera już niektórych atrybutów sprawdzania poprawności, po prostu musimy dodać kilka plików JavaScript do naszych Create.aspx Wyświetl szablon i Dodaj wiersz kodu w celu włączenia weryfikacji po stronie klienta została wykonana.</span><span class="sxs-lookup"><span data-stu-id="3870d-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="3870d-132">Z poziomu VWD naszych folder widoków/filmu go i otwórz Create.aspx.</span><span class="sxs-lookup"><span data-stu-id="3870d-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="3870d-133">Otwórz folder skryptów w Eksploratorze rozwiązań i przeciągnij następujące trzy skrypty można w ciągu &lt;head&gt; tagu.</span><span class="sxs-lookup"><span data-stu-id="3870d-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="3870d-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="3870d-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="3870d-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="3870d-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="3870d-136">Chcesz, aby te pliki skryptów, pojawią się w podanej kolejności.</span><span class="sxs-lookup"><span data-stu-id="3870d-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="3870d-137">Ponadto Dodaj pojedynczy wiersz powyżej Html.BeginForm:</span><span class="sxs-lookup"><span data-stu-id="3870d-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="3870d-138">Poniżej przedstawiono kod przedstawiony w środowisku IDE.</span><span class="sxs-lookup"><span data-stu-id="3870d-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="3870d-139">[![Filmy — Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="3870d-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="3870d-140">Uruchom aplikację ponownie odwiedzić /Movies/Create i kliknij przycisk Utwórz, bez konieczności wprowadzania żadnych danych.</span><span class="sxs-lookup"><span data-stu-id="3870d-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="3870d-141">Komunikaty o błędach pojawiają się natychmiast bez flash, że powiązane z wysyłania danych strony aż do serwera.</span><span class="sxs-lookup"><span data-stu-id="3870d-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="3870d-142">Jest tak, ponieważ platformy ASP.NET MVC jest teraz Walidacja danych wejściowych, zarówno klienta (przy użyciu języka JavaScript) i na serwerze.</span><span class="sxs-lookup"><span data-stu-id="3870d-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="3870d-143">[![Tworzenie — Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="3870d-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="3870d-144">To jest dobra wydajność!</span><span class="sxs-lookup"><span data-stu-id="3870d-144">This is looking good!</span></span> <span data-ttu-id="3870d-145">Teraz Dodajmy jedną dodatkową kolumnę w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3870d-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3870d-146">[Poprzednie](getting-started-with-mvc-part6.md)
> [dalej](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="3870d-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
