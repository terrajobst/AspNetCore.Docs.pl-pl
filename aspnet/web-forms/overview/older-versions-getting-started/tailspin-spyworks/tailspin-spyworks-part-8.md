---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Część 8: Końcowe strony, obsługa wyjątków i zawarcia | Dokumentacja firmy Microsoft'
author: JoeStagner
description: W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 8 dodaje skontaktuj się z pomocą strony, strony i wyjątków — informacje...
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 4fb0147a5cd8621e5341e4995960adf2f00fffc0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804720"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="db97f-104">Część 8: Końcowe strony, obsługa wyjątków i zawarcia</span><span class="sxs-lookup"><span data-stu-id="db97f-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="db97f-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="db97f-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="db97f-106">Tailspin Spyworks pokazuje, jak bardzo łatwo jest tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="db97f-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="db97f-107">Przedstawia on poza sposób użycia wspaniałych nowych funkcjach w ASP.NET 4 do tworzenia sklep online, m.in. zakupy wyewidencjonowanie i Administracja.</span><span class="sxs-lookup"><span data-stu-id="db97f-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="db97f-108">W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="db97f-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="db97f-109">Część 8 dodaje kontaktu strony, strony i obsługa wyjątków — informacje.</span><span class="sxs-lookup"><span data-stu-id="db97f-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="db97f-110">To jest zawarcie tej serii.</span><span class="sxs-lookup"><span data-stu-id="db97f-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a>  <span data-ttu-id="db97f-111">Skontaktuj się z strony (wysyłanie wiadomości e-mail z platformy ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="db97f-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="db97f-112">Utwórz nową stronę o nazwie ContactUs.aspx</span><span class="sxs-lookup"><span data-stu-id="db97f-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="db97f-113">Za pomocą projektanta, utwórz następującą postać biorąc ważne, aby uwzględnić ToolkitScriptManager i kontrolka edytora z AjaxdControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="db97f-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="db97f-114">.</span><span class="sxs-lookup"><span data-stu-id="db97f-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="db97f-115">Kliknij dwukrotnie przycisk "Prześlij", aby wygenerować obsługi zdarzeń kliknięcia w kodzie pliku i zaimplementować metodę, aby wysyłać informacje kontaktowe jako wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="db97f-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="db97f-116">Ten kod wymaga, że plik web.config zawiera wpis w sekcji konfiguracji, który określa serwer SMTP używany do wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="db97f-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  <span data-ttu-id="db97f-117">Informacje o stronie</span><span class="sxs-lookup"><span data-stu-id="db97f-117">About Page</span></span>

<span data-ttu-id="db97f-118">Utwórz stronę o nazwie AboutUs.aspx i niezależnie od zawartości, które chcesz dodać.</span><span class="sxs-lookup"><span data-stu-id="db97f-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a>  <span data-ttu-id="db97f-119">Globalnego programu obsługi wyjątków</span><span class="sxs-lookup"><span data-stu-id="db97f-119">Global Exception Handler</span></span>

<span data-ttu-id="db97f-120">Na koniec w całej aplikacji firma Microsoft ma zgłaszane i nieprzewidzianych okoliczności oznacza zimnych także przyczyną braku obsługi wyjątków w naszej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="db97f-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="db97f-121">Firma Microsoft nigdy nie ma nieobsługiwany wyjątek ma być wyświetlany obiekt odwiedzający witrynę sieci web.</span><span class="sxs-lookup"><span data-stu-id="db97f-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="db97f-122">Oprócz trwa panowanie komfortu nieobsługiwanych wyjątków może być również problem z zabezpieczeniami.</span><span class="sxs-lookup"><span data-stu-id="db97f-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="db97f-123">Aby rozwiązać ten problem wdrażamy globalnego programu obsługi wyjątków.</span><span class="sxs-lookup"><span data-stu-id="db97f-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="db97f-124">Aby to zrobić, otwórz plik Global.asax i zanotuj następującą obsługę zdarzeń wstępnie wygenerowane.</span><span class="sxs-lookup"><span data-stu-id="db97f-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="db97f-125">Dodaj kod, aby wdrożyć aplikację\_procedurę obsługi błędów w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="db97f-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="db97f-126">Następnie dodaj stronę o nazwie Error.aspx do rozwiązania i Dodaj następujący fragment kodu znaczników.</span><span class="sxs-lookup"><span data-stu-id="db97f-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="db97f-127">Teraz na stronie\_obciążenia wyodrębniania procedury obsługi zdarzeń, komunikaty o błędach z obiektu żądania.</span><span class="sxs-lookup"><span data-stu-id="db97f-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  <span data-ttu-id="db97f-128">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="db97f-128">Conclusion</span></span>

<span data-ttu-id="db97f-129">Zobaczyliśmy, że formularzy sieci Web platformy ASP.NET można łatwo utworzyć zaawansowane witryny sieci Web z dostępu do bazy danych, członkostwo w technologii AJAX, itp.</span><span class="sxs-lookup"><span data-stu-id="db97f-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="db97f-130">bardzo szybko.</span><span class="sxs-lookup"><span data-stu-id="db97f-130">pretty quickly.</span></span>

<span data-ttu-id="db97f-131">Miejmy nadzieję w tym samouczku przyznał Ci narzędzia, których potrzebujesz do rozpoczęcia tworzenia własnych WebForms ASP.NET aplikacji!</span><span class="sxs-lookup"><span data-stu-id="db97f-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="db97f-132">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="db97f-132">Previous</span></span>](tailspin-spyworks-part-7.md)
