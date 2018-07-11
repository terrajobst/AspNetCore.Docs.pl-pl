---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: Omówienie routingu platformy ASP.NET MVC (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'W tym samouczku Walther Autor: Stephen pokazuje, jak platforma ASP.NET MVC mapuje żądania przeglądarki do akcji kontrolera.'
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 2b43c3dca1587d79a60c5a89a451dda989403d56
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819150"
---
<a name="aspnet-mvc-routing-overview-c"></a><span data-ttu-id="c5ba7-103">Omówienie routingu platformy ASP.NET MVC (C#)</span><span class="sxs-lookup"><span data-stu-id="c5ba7-103">ASP.NET MVC Routing Overview (C#)</span></span>
====================
<span data-ttu-id="c5ba7-104">przez [Walther Autor: Stephen](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="c5ba7-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="c5ba7-105">W tym samouczku Walther Autor: Stephen pokazuje, jak platforma ASP.NET MVC mapuje żądania przeglądarki do akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>


<span data-ttu-id="c5ba7-106">W tym samouczku zostały wprowadzone do ważną funkcją każda aplikacja platformy ASP.NET MVC wywołuje *routingu platformy ASP.NET*.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="c5ba7-107">Moduł routingu platformy ASP.NET jest odpowiedzialny za mapowanie żądań przychodzących przeglądarki do określonej akcji kontrolera MVC.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="c5ba7-108">Do końca tego samouczka wiesz jak tabela tras standardowa mapuje żądania do akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="c5ba7-109">Za pomocą tabeli tras domyślne</span><span class="sxs-lookup"><span data-stu-id="c5ba7-109">Using the Default Route Table</span></span>

<span data-ttu-id="c5ba7-110">Podczas tworzenia nowej aplikacji platformy ASP.NET MVC, aplikacja jest już skonfigurowana do użycia routingu platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="c5ba7-111">Routingu platformy ASP.NET jest skonfigurowana w dwóch miejscach.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="c5ba7-112">Po pierwsze routingu platformy ASP.NET jest włączone w pliku konfiguracji sieci Web aplikacji (plik Web.config).</span><span class="sxs-lookup"><span data-stu-id="c5ba7-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="c5ba7-113">Istnieją cztery sekcje w pliku konfiguracji, które są istotne dla routingu: sekcja system.web.httpModules, sekcja system.web.httpHandlers, system.webserver.modules i system.webserver.handlers sekcji.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="c5ba7-114">Uważaj, aby nie usunąć te sekcje, ponieważ bez tych sekcji routingu nie będą już działać.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="c5ba7-115">Po drugie, i co ważniejsze tabelę tras jest tworzony w pliku Global.asax aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="c5ba7-116">Plik Global.asax to plik specjalny, który zawiera programy obsługi zdarzeń dla zdarzenia cyklu życia aplikacji ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="c5ba7-117">Tabela tras jest tworzony podczas zdarzenia rozpoczęcia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="c5ba7-118">Plik w ofercie 1 zawiera domyślny plik Global.asax dla aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="c5ba7-119">**Wyświetlanie listy 1 — Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="c5ba7-119">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

<span data-ttu-id="c5ba7-120">Gdy aplikacja MVC pierwszym uruchomieniu, aplikacja\_wywołanie metody Start().</span><span class="sxs-lookup"><span data-stu-id="c5ba7-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="c5ba7-121">Ta metoda z kolei wywołuje metodę RegisterRoutes().</span><span class="sxs-lookup"><span data-stu-id="c5ba7-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="c5ba7-122">Metoda RegisterRoutes() tworzy tabelę tras.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="c5ba7-123">Tabela routingu domyślnego zawiera jedną trasę (o nazwie domyślnej).</span><span class="sxs-lookup"><span data-stu-id="c5ba7-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="c5ba7-124">Trasa domyślna mapuje pierwszy segment adresu URL do nazwy kontrolera, drugi segment adresu URL do akcji kontrolera i segmentu trzeciego parametru o nazwie **identyfikator**.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="c5ba7-125">Wyobraź sobie, wprowadź następujący adres URL na pasku adresu przeglądarki sieci web:</span><span class="sxs-lookup"><span data-stu-id="c5ba7-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="c5ba7-126">/ Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="c5ba7-126">/Home/Index/3</span></span>

<span data-ttu-id="c5ba7-127">Trasa domyślna mapuje ten adres URL do następujących parametrów:</span><span class="sxs-lookup"><span data-stu-id="c5ba7-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="c5ba7-128">Kontroler = strona główna</span><span class="sxs-lookup"><span data-stu-id="c5ba7-128">controller = Home</span></span>

- <span data-ttu-id="c5ba7-129">Akcja = indeks</span><span class="sxs-lookup"><span data-stu-id="c5ba7-129">action = Index</span></span>

- <span data-ttu-id="c5ba7-130">id = 3</span><span class="sxs-lookup"><span data-stu-id="c5ba7-130">id = 3</span></span>

<span data-ttu-id="c5ba7-131">W przypadku żądania /Home adresu URL/indeksu/3, poniższy kod jest wykonywany:</span><span class="sxs-lookup"><span data-stu-id="c5ba7-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="c5ba7-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="c5ba7-132">HomeController.Index(3)</span></span>

<span data-ttu-id="c5ba7-133">Trasa domyślna obejmuje ustawienia domyślne dla wszystkich trzech parametrów.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="c5ba7-134">Jeśli nie podasz kontrolera, następnie kontrolera jest domyślnie wartość **Home**.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="c5ba7-135">Jeśli nie podasz akcję, parametr akcji wartość domyślna to wartość **indeksu**.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="c5ba7-136">Na koniec Jeśli nie podasz identyfikatora parametru identyfikatora wartość domyślna to ciąg pusty.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="c5ba7-137">Spójrzmy na kilka przykładów jak trasa domyślna mapuje adresy URL do akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="c5ba7-138">Wyobraź sobie, wprowadź następujący adres URL w pasku adresu przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="c5ba7-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="c5ba7-139">Domowych</span><span class="sxs-lookup"><span data-stu-id="c5ba7-139">/Home</span></span>

<span data-ttu-id="c5ba7-140">Ze względu na wartości domyślne parametrów trasy domyślne wprowadzając ten adres URL spowoduje, że metoda indeks() klasy HomeController w ofercie 2 do wywołania.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="c5ba7-141">**Wyświetlanie listy 2 - HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="c5ba7-141">**Listing 2 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

<span data-ttu-id="c5ba7-142">W ofercie 2 Klasa HomeController zawiera metodę o nazwie indeks(), który akceptuje pojedynczy parametr o nazwie identyfikatora. /Home adres URL spowoduje, że metoda indeks() nelze volat pusty ciąg jako wartość parametru identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with an empty string as the value of the Id parameter.</span></span>

<span data-ttu-id="c5ba7-143">Ze względu na sposób, że struktura MVC wywołuje akcji kontrolera /Home adresu URL dopasowuje metodę indeks() klasy HomeController w ofercie 3.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="c5ba7-144">**Wyświetlanie listy 3 - HomeController.cs (indeks akcji w przypadku braku parametrów)**</span><span class="sxs-lookup"><span data-stu-id="c5ba7-144">**Listing 3 - HomeController.cs (Index action with no parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

<span data-ttu-id="c5ba7-145">Metoda indeks() w ofercie 3 nie przyjmuje żadnych parametrów.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="c5ba7-146">/Home adres URL spowoduje, że ta metoda indeks() do wywołania.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="c5ba7-147">/Home adresu URL/indeksu/3 również wywołuje tę metodę (identyfikator jest ignorowany).</span><span class="sxs-lookup"><span data-stu-id="c5ba7-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="c5ba7-148">/Home adres URL zgodny metoda indeks() klasy HomeController w ofercie 4.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="c5ba7-149">**Wyświetlanie listy 4 - HomeController.cs (Akcja indeksu za pomocą parametru dopuszczającego wartość null)**</span><span class="sxs-lookup"><span data-stu-id="c5ba7-149">**Listing 4 - HomeController.cs (Index action with nullable parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

<span data-ttu-id="c5ba7-150">W ofercie 4 metoda indeks() ma jeden parametr liczby całkowitej.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="c5ba7-151">Ponieważ wartość parametru jest parametru dopuszczającego wartość null (może mieć wartości Null), indeks() mogą być wywoływane bez zgłaszania błędu.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-151">Because the parameter is a nullable parameter (can have the value Null), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="c5ba7-152">Na koniec wywołania metody indeks() w ofercie 5 /Home adres URL powoduje wyjątek od parametru identyfikatora *nie* parametru dopuszczającego wartość null.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="c5ba7-153">Jeśli użytkownik podejmie próbę wywołania metody indeks() otrzymasz błąd wyświetlane na rysunku 1.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="c5ba7-154">**Wyświetlanie listy 5 - HomeController.cs (Akcja indeksu za pomocą parametru Id)**</span><span class="sxs-lookup"><span data-stu-id="c5ba7-154">**Listing 5 - HomeController.cs (Index action with Id parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


<span data-ttu-id="c5ba7-155">[![Wywołanie akcji kontrolera, który oczekuje, że wartość parametru](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c5ba7-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="c5ba7-156">**Rysunek 01**: wywołanie akcji kontrolera, który oczekuje, że wartość parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](asp-net-mvc-routing-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="c5ba7-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-cs/_static/image2.png))</span></span>


<span data-ttu-id="c5ba7-157">Adres URL /Home/indeksu/3 z drugiej strony, nadaje tylko przy użyciu akcji kontrolera indeksu w ofercie 5.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="c5ba7-158">/Home/Index/3 żądanie spowoduje, że metoda indeks() nelze volat parametru Id, który ma wartość 3.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="c5ba7-159">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="c5ba7-159">Summary</span></span>

<span data-ttu-id="c5ba7-160">Celem tego samouczka było udostępnić krótkie wprowadzenie do routingu platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="c5ba7-161">Zbadaliśmy tabela routingu domyślnego, której można korzystać z nowej aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="c5ba7-162">Pokazaliśmy ci, jak trasy domyślnej mapuje adresy URL do akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c5ba7-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c5ba7-163">Next</span><span class="sxs-lookup"><span data-stu-id="c5ba7-163">Next</span></span>](understanding-action-filters-cs.md)
