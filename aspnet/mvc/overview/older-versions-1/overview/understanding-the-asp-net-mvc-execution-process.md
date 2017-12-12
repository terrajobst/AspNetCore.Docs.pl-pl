---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Opis procesu wykonywania platformy ASP.NET MVC | Dokumentacja firmy Microsoft
author: microsoft
description: "Dowiedz się, jak platforma ASP.NET MVC przetwarza żądanie przeglądarki krok po kroku."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="understanding-the-aspnet-mvc-execution-process"></a><span data-ttu-id="a0a7b-103">Opis procesu wykonywania platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="a0a7b-103">Understanding the ASP.NET MVC Execution Process</span></span>
====================
<span data-ttu-id="a0a7b-104">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a0a7b-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a0a7b-105">Dowiedz się, jak platforma ASP.NET MVC przetwarza żądanie przeglądarki krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-105">Learn how the ASP.NET MVC framework processes a browser request step-by-step.</span></span>


<span data-ttu-id="a0a7b-106">Najpierw przekazywania żądań do aplikacji sieci Web opartych na platformie ASP.NET MVC **UrlRoutingModule** obiektu, który jest moduł protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-106">Requests to an ASP.NET MVC-based Web application first pass through the **UrlRoutingModule** object, which is an HTTP module.</span></span> <span data-ttu-id="a0a7b-107">Ten moduł analizuje żądania i wykonuje wybór trasy.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-107">This module parses the request and performs route selection.</span></span> <span data-ttu-id="a0a7b-108">**UrlRoutingModule** obiektu wybiera pierwszy obiekt trasy odpowiadający bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-108">The **UrlRoutingModule** object selects the first route object that matches the current request.</span></span> <span data-ttu-id="a0a7b-109">(Obiekt trasy jest klasa implementująca **RouteBase**, i jest zwykle wystąpienia **trasy** klasy.) Jeśli trasy nie są zgodne, **UrlRoutingModule** obiekt nie robi nic i umożliwia przełączyć się na regularne żądania ASP.NET i IIS przetwarzania żądania.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-109">(A route object is a class that implements **RouteBase**, and is typically an instance of the **Route** class.) If no routes match, the **UrlRoutingModule** object does nothing and lets the request fall back to the regular ASP.NET or IIS request processing.</span></span>

<span data-ttu-id="a0a7b-110">Z wybranego **trasy** obiektu **UrlRoutingModule** uzyskuje obiekt **IRouteHandler** obiektu, z którym skojarzony jest **trasy**obiektu.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-110">From the selected **Route** object, the **UrlRoutingModule** object obtains the **IRouteHandler** object that is associated with the **Route** object.</span></span> <span data-ttu-id="a0a7b-111">Zazwyczaj w aplikacji MVC, będzie to wystąpienie **MvcRouteHandler**.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-111">Typically, in an MVC application, this will be an instance of **MvcRouteHandler**.</span></span> <span data-ttu-id="a0a7b-112">**IRouteHandler** tworzy wystąpienie **IHttpHandler** obiektu i przekazuje je **IHttpContext** obiektu.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-112">The **IRouteHandler** instance creates an **IHttpHandler** object and passes it the **IHttpContext** object.</span></span> <span data-ttu-id="a0a7b-113">Domyślnie **IHttpHandler** wystąpienie jest MVC **MvcHandler** obiektu.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-113">By default, the **IHttpHandler** instance for MVC is the **MvcHandler** object.</span></span> <span data-ttu-id="a0a7b-114">**MvcHandler** obiektu następnie wybiera kontrolera, który ostatecznie obsłuży żądanie.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-114">The **MvcHandler** object then selects the controller that will ultimately handle the request.</span></span>

> [!NOTE]
> <span data-ttu-id="a0a7b-115">Po uruchomieniu aplikacji sieci Web programu ASP.NET MVC w usługach IIS 7.0, bez rozszerzenia nazwy pliku jest wymagana dla projektów MVC.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-115">When an ASP.NET MVC Web application runs in IIS 7.0, no file name extension is required for MVC projects.</span></span> <span data-ttu-id="a0a7b-116">W usługach IIS 6.0, program obsługi wymaga jednak mapowania rozszerzenia nazwy pliku MVC ASP.NET ISAPI DLL.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-116">However, in IIS 6.0, the handler requires that you map the .mvc file name extension to the ASP.NET ISAPI DLL.</span></span>


<span data-ttu-id="a0a7b-117">Moduł i obsługi są punkty wejścia do platformę ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-117">The module and handler are the entry points to the ASP.NET MVC framework.</span></span> <span data-ttu-id="a0a7b-118">Wykonują następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="a0a7b-118">They perform the following actions:</span></span>

- <span data-ttu-id="a0a7b-119">Wybierz odpowiedni kontroler w aplikacji sieci Web MVC.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-119">Select the appropriate controller in an MVC Web application.</span></span>
- <span data-ttu-id="a0a7b-120">Uzyskaj wystąpienia określonego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-120">Obtain a specific controller instance.</span></span>
- <span data-ttu-id="a0a7b-121">Wywołanie kontrolera **Execute** metody.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-121">Call the controller's **Execute** method.</span></span>

<span data-ttu-id="a0a7b-122">Poniższa lista zawiera etapy wykonywania dla projektu sieci Web MVC:</span><span class="sxs-lookup"><span data-stu-id="a0a7b-122">The following lists the stages of execution for an MVC Web project:</span></span>

- <span data-ttu-id="a0a7b-123">Odbieranie pierwsze żądanie dla aplikacji</span><span class="sxs-lookup"><span data-stu-id="a0a7b-123">Receive first request for the application</span></span> 

    - <span data-ttu-id="a0a7b-124">W pliku Global.asax **trasy** obiekty są dodawane do **stan** obiektu.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-124">In the Global.asax file, **Route** objects are added to the **RouteTable** object.</span></span>
- <span data-ttu-id="a0a7b-125">Wykonaj routingu</span><span class="sxs-lookup"><span data-stu-id="a0a7b-125">Perform routing</span></span> 

    - <span data-ttu-id="a0a7b-126">**UrlRoutingModule** modułu używa pierwszego dopasowywania **trasy** obiektu w **stan** kolekcji, aby utworzyć **RouteData** obiekt, który następnie używane w celu utworzenia **RequestContext** (**IHttpContext**) obiektu.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-126">The **UrlRoutingModule** module uses the first matching **Route** object in the **RouteTable** collection to create the **RouteData** object, which it then uses to create a **RequestContext** (**IHttpContext**) object.</span></span>
- <span data-ttu-id="a0a7b-127">Utwórz program obsługi żądania MVC</span><span class="sxs-lookup"><span data-stu-id="a0a7b-127">Create MVC request handler</span></span> 

    - <span data-ttu-id="a0a7b-128">**MvcRouteHandler** obiektu tworzy wystąpienie **MvcHandler** klasy i przekazuje je **RequestContext** wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-128">The **MvcRouteHandler** object creates an instance of the **MvcHandler** class and passes it the **RequestContext** instance.</span></span>
- <span data-ttu-id="a0a7b-129">Tworzenie kontrolera</span><span class="sxs-lookup"><span data-stu-id="a0a7b-129">Create controller</span></span> 

    - <span data-ttu-id="a0a7b-130">**MvcHandler** obiekt używa **RequestContext** wystąpienia, aby zidentyfikować **IControllerFactory** obiektu (zazwyczaj wystąpienia  **DefaultControllerFactory** klasy) można utworzyć wystąpienia kontrolera z.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-130">The **MvcHandler** object uses the **RequestContext** instance to identify the **IControllerFactory** object (typically an instance of the **DefaultControllerFactory** class) to create the controller instance with.</span></span>
- <span data-ttu-id="a0a7b-131">Kontroler EXECUTE - **MvcHandler** wystąpienia wywołuje kontroler s **Execute** — metoda.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-131">Execute controller - The **MvcHandler** instance calls the controller s **Execute** method.</span></span> |
- <span data-ttu-id="a0a7b-132">Wywołaj akcję</span><span class="sxs-lookup"><span data-stu-id="a0a7b-132">Invoke action</span></span> 

    - <span data-ttu-id="a0a7b-133">Większość kontrolerów dziedziczyć **kontrolera** klasy podstawowej.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-133">Most controllers inherit from the **Controller** base class.</span></span> <span data-ttu-id="a0a7b-134">Kontrolerów z tym **ControllerActionInvoker** obiekt, który jest skojarzony z kontrolerem określa metodę akcji klasy kontrolera do wywołania, a następnie wywołuje tę metodę.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-134">For controllers that do so, the **ControllerActionInvoker** object that is associated with the controller determines which action method of the controller class to call, and then calls that method.</span></span>
- <span data-ttu-id="a0a7b-135">Wynik wykonania</span><span class="sxs-lookup"><span data-stu-id="a0a7b-135">Execute result</span></span> 

    - <span data-ttu-id="a0a7b-136">Metoda typowych akcji może odbierać dane wejściowe użytkownika, przygotowanie danych właściwą odpowiedź, a następnie wykonującą wynik zwróciła typ wyniku.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-136">A typical action method might receive user input, prepare the appropriate response data, and then execute the result by returning a result type.</span></span> <span data-ttu-id="a0a7b-137">Typy wbudowane wyników, które mogą być wykonywane są następujące: **ViewResult** (który renderuje widok i jest typu wyniku większość — często używane), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, i **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="a0a7b-137">The built-in result types that can be executed include the following: **ViewResult** (which renders a view and is the most-often used result type), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, and **EmptyResult**.</span></span>
