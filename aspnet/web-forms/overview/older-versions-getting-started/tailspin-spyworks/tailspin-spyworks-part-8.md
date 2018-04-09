---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Część 8: Końcowe strony, obsługa wyjątków i zawarcia | Dokumentacja firmy Microsoft'
author: JoeStagner
description: Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 8 dodaje kontaktu strony, strony i wyjątków — informacje...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: f82294aab0616012393cf3e10f932f6d1ad0cdb6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="f19c3-104">Część 8: Końcowe strony, obsługa wyjątków i zawarcia</span><span class="sxs-lookup"><span data-stu-id="f19c3-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="f19c3-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="f19c3-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="f19c3-106">Tailspin Spyworks pokazano, jak bardzo proste jest tworzenie zaawansowanych, skalowalnych aplikacji dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="f19c3-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="f19c3-107">Przedstawia on poza jak nowe, fantastyczne funkcje programu ASP.NET 4 do tworzenia sklepu online, łącznie z zakupów, wyewidencjonowania i administracji.</span><span class="sxs-lookup"><span data-stu-id="f19c3-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="f19c3-108">Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f19c3-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="f19c3-109">Część 8 dodaje kontaktu strony, strony i obsługa wyjątków — informacje.</span><span class="sxs-lookup"><span data-stu-id="f19c3-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="f19c3-110">Jest to zawarcia serii.</span><span class="sxs-lookup"><span data-stu-id="f19c3-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a>  <span data-ttu-id="f19c3-111">Skontaktuj się z strony (wysyłanie wiadomości e-mail z platformy ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="f19c3-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="f19c3-112">Utwórz nową stronę o nazwie ContactUs.aspx</span><span class="sxs-lookup"><span data-stu-id="f19c3-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="f19c3-113">Przy użyciu narzędzia Projektant, utwórz następującą postać uwzględnieniu specjalne ToolkitScriptManager i kontrolce edytora z AjaxdControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="f19c3-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="f19c3-114">.</span><span class="sxs-lookup"><span data-stu-id="f19c3-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="f19c3-115">Kliknij dwukrotnie przycisk "Zatwierdź" Generowanie obsługi zdarzeń kliknięcia w kodzie pliku i zaimplementuj metodę można wysłać informacji kontaktowych jako wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="f19c3-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="f19c3-116">Ten kod wymaga, aby plik web.config zawiera wpis w sekcji konfiguracji, który określa serwer SMTP używany do wysyłania poczty.</span><span class="sxs-lookup"><span data-stu-id="f19c3-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  <span data-ttu-id="f19c3-117">Strona — informacje</span><span class="sxs-lookup"><span data-stu-id="f19c3-117">About Page</span></span>

<span data-ttu-id="f19c3-118">Utwórz stronę o nazwie AboutUs.aspx i niezależnie od zawartości, które chcesz dodać.</span><span class="sxs-lookup"><span data-stu-id="f19c3-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a>  <span data-ttu-id="f19c3-119">Globalnego programu obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="f19c3-119">Global Exception Handler</span></span>

<span data-ttu-id="f19c3-120">Ponadto w całej aplikacji ma możemy zgłaszane wyjątki i brak nieprzewidzianych okoliczności to zimnych również Przyczyna nieobsługiwanych wyjątków w naszej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="f19c3-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="f19c3-121">Firma Microsoft nigdy nie mają nieobsługiwany wyjątek ma być wyświetlony dla obiekt odwiedzający witrynę sieci web.</span><span class="sxs-lookup"><span data-stu-id="f19c3-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="f19c3-122">Oprócz trwa olbrzymich użytkowników nieobsługiwanych wyjątków może być również problem z zabezpieczeniami.</span><span class="sxs-lookup"><span data-stu-id="f19c3-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="f19c3-123">Aby rozwiązać ten problem wprowadzimy globalnego programu obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="f19c3-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="f19c3-124">Aby to zrobić, otwórz plik Global.asax i zanotuj następujące programu obsługi zdarzeń wstępnie wygenerowane.</span><span class="sxs-lookup"><span data-stu-id="f19c3-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="f19c3-125">Dodaj kod, aby wdrożyć aplikację\_program obsługi błędów w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="f19c3-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="f19c3-126">Następnie dodaj stronę o nazwie Error.aspx do rozwiązania i Dodaj następujący fragment kodu znaczników.</span><span class="sxs-lookup"><span data-stu-id="f19c3-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="f19c3-127">Teraz na stronie\_załadować wyodrębniania programu obsługi zdarzeń komunikaty o błędach z obiektu żądanie.</span><span class="sxs-lookup"><span data-stu-id="f19c3-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  <span data-ttu-id="f19c3-128">Zawierania</span><span class="sxs-lookup"><span data-stu-id="f19c3-128">Conclusion</span></span>

<span data-ttu-id="f19c3-129">Firma Microsoft w tym samouczku czy ASP.NET WebForms można łatwo utworzyć zaawansowane witryny sieci Web z dostępem do bazy danych, członkostwa, AJAX, itp.</span><span class="sxs-lookup"><span data-stu-id="f19c3-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="f19c3-130">bardzo szybko.</span><span class="sxs-lookup"><span data-stu-id="f19c3-130">pretty quickly.</span></span>

<span data-ttu-id="f19c3-131">Miejmy nadzieję, że w tym samouczku przyznał Ci narzędzia, które należy rozpocząć tworzenie własnych formularzy sieci Web ASP.NET aplikacji!</span><span class="sxs-lookup"><span data-stu-id="f19c3-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f19c3-132">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="f19c3-132">Previous</span></span>](tailspin-spyworks-part-7.md)
