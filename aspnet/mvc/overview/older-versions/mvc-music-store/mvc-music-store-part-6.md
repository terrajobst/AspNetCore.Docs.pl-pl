---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Część 6: Przy użyciu adnotacje danych na potrzeby weryfikacji modelu | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 6 obejmuje korzystanie z adnotacji danych dla modelu V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: ca0ac2ca909f838f1c91e6cc01b8aafa90c0b193
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397659"
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="0e191-104">Część 6: Przy użyciu adnotacje danych na potrzeby weryfikacji modelu</span><span class="sxs-lookup"><span data-stu-id="0e191-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="0e191-105">przez [Galloway'em Jon](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="0e191-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="0e191-106">MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="0e191-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="0e191-107">MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="0e191-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="0e191-108">W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="0e191-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="0e191-109">Część 6 obejmuje przy użyciu adnotacje danych na potrzeby weryfikacji modelu.</span><span class="sxs-lookup"><span data-stu-id="0e191-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="0e191-110">Mamy poważnym problemem z naszych formularzami tworzyć i edytować: nie robią wszystkich sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="0e191-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="0e191-111">Możemy to zrobić na przykład pozostawić wymagane pola puste lub liter, wpisz w polu Cena, a pierwszy błąd, który zobaczymy to z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0e191-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="0e191-112">Firma Microsoft można łatwo dodać sprawdzanie poprawności do naszej aplikacji, dodając adnotacje danych do naszych zajęć modelu.</span><span class="sxs-lookup"><span data-stu-id="0e191-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="0e191-113">Adnotacje danych umożliwiają nam opisano reguły, którą chcemy stosowane do naszych właściwości modelu i platformy ASP.NET MVC zajmie się ich wymuszania i wyświetlania odpowiednie komunikaty naszych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="0e191-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="0e191-114">Dodawanie walidacji do naszych albumu formularzy</span><span class="sxs-lookup"><span data-stu-id="0e191-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="0e191-115">Użyjemy następujących atrybutów danych adnotacji:</span><span class="sxs-lookup"><span data-stu-id="0e191-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="0e191-116">**Wymagane** — wskazuje, że właściwość jest polem wymaganym</span><span class="sxs-lookup"><span data-stu-id="0e191-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="0e191-117">**DisplayName** — Określa tekst, chcemy, aby używane pola formularza i komunikatów dotyczących sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="0e191-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="0e191-118">**StringLength** — określa maksymalną długość pola ciągu</span><span class="sxs-lookup"><span data-stu-id="0e191-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="0e191-119">**Zakres** — zapewnia maksymalne i minimalne wartości pola numerycznego</span><span class="sxs-lookup"><span data-stu-id="0e191-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="0e191-120">**Powiąż** — zawiera listę pól, aby wykluczyć lub uwzględnić podczas tworzenia wiązania parametru lub formularz wartości do właściwości modelu</span><span class="sxs-lookup"><span data-stu-id="0e191-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="0e191-121">**ScaffoldColumn** — umożliwia ukrycie pola w edytorze formularzy</span><span class="sxs-lookup"><span data-stu-id="0e191-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="0e191-122">*Uwaga: Aby uzyskać więcej informacji o weryfikacji modelu przy użyciu adnotacji danych atrybutów, zobacz dokumentację MSDN w*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="0e191-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="0e191-123">Otwórz klasę albumu i dodaj następującą *przy użyciu* instrukcji u góry.</span><span class="sxs-lookup"><span data-stu-id="0e191-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="0e191-124">Następnie zaktualizuj właściwości, aby dodać wyświetlanie i sprawdzanie poprawności atrybutów, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="0e191-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="0e191-125">Są nam również zmieniliśmy wykonawcy i gatunku do właściwości wirtualnego.</span><span class="sxs-lookup"><span data-stu-id="0e191-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="0e191-126">Dzięki temu Entity Framework z opóźnieniem ładować je zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="0e191-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="0e191-127">Po mających te atrybuty są dodawane do nasz model fotograficzne, nasze ekranu tworzyć i edytować natychmiast rozpocząć sprawdzanie poprawności pól i przy użyciu nazw wyświetlanych Wybraliśmy (np. albumu grafikę adresu Url zamiast AlbumArtUrl).</span><span class="sxs-lookup"><span data-stu-id="0e191-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="0e191-128">Uruchom aplikację, a następnie przejdź do /StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="0e191-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="0e191-129">Następnie możemy zepsuć niektórych reguł sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="0e191-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="0e191-130">Wprowadź cenie 0, a tytuł jest pusta.</span><span class="sxs-lookup"><span data-stu-id="0e191-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="0e191-131">Po kliknięciu przycisku Utwórz widzimy formularzu wyświetlane przy użyciu komunikatów o błędach weryfikacji wyświetlane pola, które nie spełnia warunków reguły sprawdzania poprawności, gdy zdefiniowaliśmy.</span><span class="sxs-lookup"><span data-stu-id="0e191-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="0e191-132">Testowanie poprawności po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="0e191-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="0e191-133">Weryfikacja po stronie serwera jest bardzo ważne z perspektywy aplikacji, ponieważ użytkownicy mogą omijać weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="0e191-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="0e191-134">Jednakże formularze sieci Web, które tylko zaimplementować weryfikację po stronie serwera następującej liczby etapów stwierdzono trzy istotne problemy.</span><span class="sxs-lookup"><span data-stu-id="0e191-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="0e191-135">Użytkownik będzie musiał czekać do formularza, który ma zostać opublikowany, sprawdzania poprawności na serwerze, a odpowiedź do wysłania do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="0e191-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="0e191-136">Użytkownik nie natychmiast otrzymać opinię podczas Popraw polem, tak, aby teraz przekazuje reguł sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="0e191-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="0e191-137">Firma Microsoft jest marnowania zasobów serwera, aby wykonać logikę weryfikacji zamiast korzystanie z przeglądarki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0e191-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="0e191-138">Na szczęście szablony szkieletu ASP.NET MVC 3 mają weryfikacji po stronie klienta wbudowane, wymagania żadne dodatkowe czynności w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="0e191-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="0e191-139">Wpisz pojedynczą literą, w polu Tytuł spełnia wymagania sprawdzania poprawności, więc komunikat sprawdzania poprawności jest od razu usunięte.</span><span class="sxs-lookup"><span data-stu-id="0e191-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> <span data-ttu-id="0e191-140">[Poprzednie](mvc-music-store-part-5.md)
> [dalej](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="0e191-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
