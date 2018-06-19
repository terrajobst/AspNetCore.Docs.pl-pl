---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: Omówienie routingu ASP.NET MVC (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: W tym samouczku Stephen Walther pokazuje sposób platforma ASP.NET MVC mapowania żądania przeglądarki akcji kontrolera.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 3de0e21552a4aa03aa21f21a4e26028f1475f3e9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879209"
---
<a name="aspnet-mvc-routing-overview-vb"></a><span data-ttu-id="a5602-103">Omówienie routingu ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="a5602-103">ASP.NET MVC Routing Overview (VB)</span></span>
====================
<span data-ttu-id="a5602-104">przez [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="a5602-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="a5602-105">W tym samouczku Stephen Walther pokazuje sposób platforma ASP.NET MVC mapowania żądania przeglądarki akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a5602-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>


<span data-ttu-id="a5602-106">W tym samouczku są wprowadzane do ważna cecha każda aplikacja platformy ASP.NET MVC wywołuje *routingu platformy ASP.NET*.</span><span class="sxs-lookup"><span data-stu-id="a5602-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="a5602-107">Moduł routingu platformy ASP.NET jest odpowiedzialny za mapowanie przychodzących żądań przeglądarki do określonej akcji kontrolera MVC.</span><span class="sxs-lookup"><span data-stu-id="a5602-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="a5602-108">Na koniec tego samouczka będzie zrozumiałe, jak tabela tras standardowe mapuje żądania do akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a5602-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="a5602-109">Korzystając z tabeli trasy domyślnej</span><span class="sxs-lookup"><span data-stu-id="a5602-109">Using the Default Route Table</span></span>

<span data-ttu-id="a5602-110">Podczas tworzenia nowej aplikacji ASP.NET MVC aplikacji jest już skonfigurowana do używania routingu platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a5602-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="a5602-111">ASP.NET Routing jest skonfigurowana w dwóch miejscach.</span><span class="sxs-lookup"><span data-stu-id="a5602-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="a5602-112">Po pierwsze proces routingu platformy ASP.NET jest włączone w pliku konfiguracji aplikacji sieci Web (plik Web.config).</span><span class="sxs-lookup"><span data-stu-id="a5602-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="a5602-113">Istnieją cztery sekcje w pliku konfiguracji, które mają zastosowanie do routingu: sekcja system.web.httpModules, sekcji system.web.httpHandlers sekcji system.webserver.modules i system.webserver.handlers sekcji.</span><span class="sxs-lookup"><span data-stu-id="a5602-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="a5602-114">Uważaj, aby nie usunąć tych sekcji, ponieważ bez tych sekcji routingu przestanie działać.</span><span class="sxs-lookup"><span data-stu-id="a5602-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="a5602-115">Po drugie, a ważniejsze tabelę tras jest tworzony w pliku Global.asax aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a5602-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="a5602-116">Plik Global.asax to specjalny plik, który zawiera programy obsługi zdarzeń dla zdarzenia cyklu życia aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a5602-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="a5602-117">Tabela tras jest tworzona podczas zdarzenia uruchomić aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a5602-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="a5602-118">Plik w 1 Lista zawiera domyślne pliku Global.asax aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a5602-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="a5602-119">**Wyświetlanie listy 1 - Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="a5602-119">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

<span data-ttu-id="a5602-120">Gdy aplikacji MVC uruchomieniu aplikacji\_Start() metoda jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="a5602-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="a5602-121">Ta metoda z kolei wywołuje metodę RegisterRoutes().</span><span class="sxs-lookup"><span data-stu-id="a5602-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="a5602-122">Metoda RegisterRoutes() tworzy tabelę tras.</span><span class="sxs-lookup"><span data-stu-id="a5602-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="a5602-123">Tabela tras domyślnych zawiera jedną trasę (o nazwie domyślnej).</span><span class="sxs-lookup"><span data-stu-id="a5602-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="a5602-124">Trasa domyślna mapuje pierwszy segment adresu URL do nazwy kontrolera, drugi segment adresu URL do akcji kontrolera i segmentu trzeciego parametru o nazwie **identyfikator**.</span><span class="sxs-lookup"><span data-stu-id="a5602-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="a5602-125">Wyobraź sobie, wprowadź następujący adres URL na pasku adresu przeglądarki sieci web:</span><span class="sxs-lookup"><span data-stu-id="a5602-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="a5602-126">/ Głównej/indeks/3</span><span class="sxs-lookup"><span data-stu-id="a5602-126">/Home/Index/3</span></span>

<span data-ttu-id="a5602-127">Trasa domyślna mapuje ten adres URL do następujących parametrów:</span><span class="sxs-lookup"><span data-stu-id="a5602-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="a5602-128">Kontroler = Home</span><span class="sxs-lookup"><span data-stu-id="a5602-128">controller = Home</span></span>

- <span data-ttu-id="a5602-129">Akcja = indeks</span><span class="sxs-lookup"><span data-stu-id="a5602-129">action = Index</span></span>

- <span data-ttu-id="a5602-130">id = 3</span><span class="sxs-lookup"><span data-stu-id="a5602-130">id = 3</span></span>

<span data-ttu-id="a5602-131">Gdy użytkownik żąda adresu URL /Home/indeks/3, jest wykonywany następujący kod:</span><span class="sxs-lookup"><span data-stu-id="a5602-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="a5602-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="a5602-132">HomeController.Index(3)</span></span>

<span data-ttu-id="a5602-133">Trasa domyślna obejmuje ustawienia domyślne dla wszystkich trzech parametrów.</span><span class="sxs-lookup"><span data-stu-id="a5602-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="a5602-134">Jeśli nie zostanie podane kontrolera, następnie parametr kontrolera domyślnie przyjmowana jest wartość **Home**.</span><span class="sxs-lookup"><span data-stu-id="a5602-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="a5602-135">Jeśli akcja nie zostanie podane, parametr akcji domyślnie przyjmowana jest wartość **indeksu**.</span><span class="sxs-lookup"><span data-stu-id="a5602-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="a5602-136">Ponadto jeśli identyfikator nie zostanie podane, parametru identyfikatora domyślnie pusty ciąg.</span><span class="sxs-lookup"><span data-stu-id="a5602-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="a5602-137">Oto kilka przykładów sposobu trasy domyślnej mapowania adresów URL do akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a5602-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="a5602-138">Wyobraź sobie, wprowadź następujący adres URL na pasku adresu przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="a5602-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="a5602-139">Domowych</span><span class="sxs-lookup"><span data-stu-id="a5602-139">/Home</span></span>

<span data-ttu-id="a5602-140">Ze względu na wartości domyślne parametrów trasy domyślne wprowadzić ten adres URL spowoduje, że metoda indeks() klasy HomeController wyświetlania 2 do wywołania.</span><span class="sxs-lookup"><span data-stu-id="a5602-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="a5602-141">**Wyświetlanie listy 2 - HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="a5602-141">**Listing 2 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

<span data-ttu-id="a5602-142">Wyświetlanie 2 Klasa HomeController zawiera metodę o nazwie indeks(), który przyjmuje jeden parametr o nazwie identyfikatora. /Home adres URL powoduje, że metoda indeks() ma być wywoływana na wartość Nothing jako wartość parametru identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="a5602-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with the value Nothing as the value of the Id parameter.</span></span>

<span data-ttu-id="a5602-143">Ze względu na sposób, że struktura MVC wywołuje akcji kontrolera /Home adres URL zgodna metoda indeks() klasy HomeController w 3 wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="a5602-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="a5602-144">**Wyświetlanie listy 3 - HomeController.vb (Akcja indeks parametru nie)**</span><span class="sxs-lookup"><span data-stu-id="a5602-144">**Listing 3 - HomeController.vb (Index action with no parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

<span data-ttu-id="a5602-145">Metoda indeks() w 3 wyświetlania nie akceptuje żadnych parametrów.</span><span class="sxs-lookup"><span data-stu-id="a5602-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="a5602-146">/Home adres URL spowoduje, że ta metoda indeks() do wywołania.</span><span class="sxs-lookup"><span data-stu-id="a5602-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="a5602-147">Adres URL /Home/indeks/3 również wywołuje tę metodę (identyfikator zostanie zignorowana).</span><span class="sxs-lookup"><span data-stu-id="a5602-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="a5602-148">/Home adres URL zgodny metody indeks() klasy HomeController w listę 4.</span><span class="sxs-lookup"><span data-stu-id="a5602-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="a5602-149">**Wyświetlanie listy 4 - HomeController.vb (akcji indeksu z parametru dopuszczającego wartość null)**</span><span class="sxs-lookup"><span data-stu-id="a5602-149">**Listing 4 - HomeController.vb (Index action with nullable parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

<span data-ttu-id="a5602-150">W przypadku wyświetlania 4 metoda indeks() ma jeden parametr.</span><span class="sxs-lookup"><span data-stu-id="a5602-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="a5602-151">Ponieważ parametr ma wartość null parametru (może mieć wartości Nothing), indeks() można wywołać bez zgłaszania błędu.</span><span class="sxs-lookup"><span data-stu-id="a5602-151">Because the parameter is a nullable parameter (can have the value Nothing), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="a5602-152">Na koniec wywołania metody indeks() listę 5 z /Home adres URL powoduje zgłoszenie wyjątku od parametru identyfikatora *nie jest* parametru dopuszczającego wartość null.</span><span class="sxs-lookup"><span data-stu-id="a5602-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="a5602-153">Jeśli próba wywołania metody indeks() otrzymasz błąd wyświetlane na rysunku 1.</span><span class="sxs-lookup"><span data-stu-id="a5602-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="a5602-154">**Wyświetlanie listy 5 - HomeController.vb (akcji indeksu z parametrem Id)**</span><span class="sxs-lookup"><span data-stu-id="a5602-154">**Listing 5 - HomeController.vb (Index action with Id parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


<span data-ttu-id="a5602-155">[![Wywoływanie akcji kontrolera, która oczekuje wartości parametru](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a5602-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="a5602-156">**Rysunek 01**: wywoływania akcji kontrolera, która oczekuje wartości parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](asp-net-mvc-routing-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="a5602-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="a5602-157">Adres URL /Home/indeks/3 z drugiej strony, działa bez problemu z akcji kontrolera indeksu w listę 5.</span><span class="sxs-lookup"><span data-stu-id="a5602-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="a5602-158">/Home/Index/3 żądania powoduje, że metoda indeks() ma być wywoływana z parametru identyfikatora, który ma wartość 3.</span><span class="sxs-lookup"><span data-stu-id="a5602-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="a5602-159">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="a5602-159">Summary</span></span>

<span data-ttu-id="a5602-160">Celem tego samouczka został dostarczają krótkie wprowadzenie do routingu platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a5602-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="a5602-161">Firma Microsoft zbadać tabeli trasy domyślnej, której można korzystać z nowej aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a5602-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="a5602-162">Przedstawiono sposób trasy domyślnej mapowania adresów URL akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a5602-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a5602-163">[Poprzednie](creating-an-action-cs.md)
> [dalej](understanding-action-filters-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a5602-163">[Previous](creating-an-action-cs.md)
[Next](understanding-action-filters-vb.md)</span></span>
