---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Część 6: Korzystanie z adnotacji danych do sprawdzania poprawności modelu | Dokumentacja firmy Microsoft'
author: jongalloway
description: Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 6 obejmuje za pomocą adnotacji danych dla modelu V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 328eccb4324bb10a7e8dec819a70129fc14c42c4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="36cc8-104">Część 6: Przy użyciu adnotacje danych na potrzeby sprawdzania poprawności modelu</span><span class="sxs-lookup"><span data-stu-id="36cc8-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="36cc8-105">przez [Galloway Jan](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="36cc8-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="36cc8-106">Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36cc8-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="36cc8-107">Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="36cc8-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="36cc8-108">Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu.</span><span class="sxs-lookup"><span data-stu-id="36cc8-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="36cc8-109">Część 6 obejmuje za pomocą adnotacji danych do weryfikacji modelu.</span><span class="sxs-lookup"><span data-stu-id="36cc8-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="36cc8-110">Mamy problem główne z naszych tworzenie i edytowanie formularzy: nie robią wszystkich sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="36cc8-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="36cc8-111">Możemy czynności, takie jak pozostaw puste wymagane pola lub typu litery w polu Cena i jest pierwszy błąd, który zajmiemy się z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="36cc8-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="36cc8-112">Firma Microsoft łatwe dodawanie walidacji do naszej aplikacji przez dodanie adnotacji danych do naszej klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="36cc8-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="36cc8-113">Adnotacje danych umożliwiają opisano reguły, którą chcemy udostępnić stosowane do naszej właściwości modelu i ASP.NET MVC zajmie się ich wymuszania i wyświetlanie odpowiednie wiadomości do użytkowników.</span><span class="sxs-lookup"><span data-stu-id="36cc8-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="36cc8-114">Dodawanie walidacji do naszej albumu formularzy</span><span class="sxs-lookup"><span data-stu-id="36cc8-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="36cc8-115">Użyjemy następujące atrybuty adnotacji danych:</span><span class="sxs-lookup"><span data-stu-id="36cc8-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="36cc8-116">**Wymagane** — wskazuje, że właściwość jest polem obowiązkowym</span><span class="sxs-lookup"><span data-stu-id="36cc8-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="36cc8-117">**Nazwa wyświetlana** — definiuje tekstu, firma Microsoft ma być użyty na pola formularza i komunikatów dotyczących sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="36cc8-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="36cc8-118">**StringLength** — określa maksymalną długość pola ciągu</span><span class="sxs-lookup"><span data-stu-id="36cc8-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="36cc8-119">**Zakres** — zapewnia maksymalne i minimalne wartości pola numerycznego</span><span class="sxs-lookup"><span data-stu-id="36cc8-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="36cc8-120">**Powiąż** — zawiera listę pól do wykluczanie lub uwzględnianie podczas wiązania parametru lub formularza wartości właściwości modelu</span><span class="sxs-lookup"><span data-stu-id="36cc8-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="36cc8-121">**ScaffoldColumn** — umożliwia ukrywanie pól z edytora formularzy</span><span class="sxs-lookup"><span data-stu-id="36cc8-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="36cc8-122">*Uwaga: Więcej informacji o weryfikacji modelu przy użyciu adnotacji danych atrybutów, w dokumentacji MSDN w*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="36cc8-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="36cc8-123">Otwórz klasy albumu i dodaj następujące *przy użyciu* instrukcje do góry.</span><span class="sxs-lookup"><span data-stu-id="36cc8-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="36cc8-124">Następnie zaktualizuj właściwości, aby dodać atrybuty ekranu i sprawdzania poprawności, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="36cc8-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="36cc8-125">Są nam również zmieniono wykonawcy i Genre do właściwości wirtualnych.</span><span class="sxs-lookup"><span data-stu-id="36cc8-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="36cc8-126">Dzięki temu Entity Framework do opóźnionego ładowania ich w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="36cc8-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="36cc8-127">Po o dodać tych atrybutów do modelu albumu, naszych ekranu tworzenie i edytowanie natychmiast rozpocząć sprawdzanie poprawności pól i przy użyciu nazw wyświetlanych możemy wybraną (np. albumów graficznych adresu Url zamiast AlbumArtUrl).</span><span class="sxs-lookup"><span data-stu-id="36cc8-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="36cc8-128">Uruchom aplikację i przejdź do /StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="36cc8-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="36cc8-129">Następnie firma Microsoft będzie Podziel niektórych reguł sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="36cc8-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="36cc8-130">Wprowadź wartości 0 i tytuł jest pusta.</span><span class="sxs-lookup"><span data-stu-id="36cc8-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="36cc8-131">Po kliknięciu przycisku Utwórz przycisk, zostanie wyświetlone wyświetlane komunikaty o błędach weryfikacji przedstawiający pola, które nie spełnia warunków reguły sprawdzania poprawności się, że ma zdefiniowanego formularza.</span><span class="sxs-lookup"><span data-stu-id="36cc8-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="36cc8-132">Testowanie weryfikacji po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="36cc8-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="36cc8-133">Weryfikacja po stronie serwera jest bardzo ważne z perspektywy aplikacji, ponieważ użytkownicy mogą omijać weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="36cc8-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="36cc8-134">Formularze sieci Web, które tylko zaimplementować weryfikację po stronie serwera mogą jednak zachowywać trzy istotne problemy.</span><span class="sxs-lookup"><span data-stu-id="36cc8-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="36cc8-135">Użytkownik będzie musiał odczekaj formularza do zaksięgowania zweryfikowane na serwerze, a odpowiedź do wysłania do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="36cc8-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="36cc8-136">Użytkownik nie otrzymywać natychmiast uzyskuje opinie, podczas ich Popraw pola tak, aby teraz przekazuje reguł sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="36cc8-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="36cc8-137">Firma Microsoft tym jak zmarnowała zasobów serwera w celu wykonywania logiki sprawdzania poprawności zamiast korzystania z przeglądarki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="36cc8-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="36cc8-138">Na szczęście szablony szkieletu ASP.NET MVC 3 mają weryfikacji po stronie klienta wbudowane, wymagających ani żadne dodatkowe czynności.</span><span class="sxs-lookup"><span data-stu-id="36cc8-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="36cc8-139">Wpisz pojedyncze litery w polu Tytuł spełnia wymagań sprawdzania poprawności, więc komunikatu weryfikacji jest od razu usunięte.</span><span class="sxs-lookup"><span data-stu-id="36cc8-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> <span data-ttu-id="36cc8-140">[Poprzednie](mvc-music-store-part-5.md)
> [dalej](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="36cc8-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
