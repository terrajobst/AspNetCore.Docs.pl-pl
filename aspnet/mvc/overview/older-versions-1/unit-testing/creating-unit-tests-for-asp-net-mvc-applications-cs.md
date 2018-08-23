---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
title: Tworzenie testów jednostkowych dla aplikacji ASP.NET MVC (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Informacje o sposobie tworzenia testów jednostkowych dla akcji kontrolera. W tym samouczku Walther Autor: Stephen pokazuje, jak sprawdzić, czy akcja kontrolera zwraca częśći...'
ms.author: riande
ms.date: 08/19/2008
ms.assetid: d3a270b9-d7b1-47f2-8775-fc3beb518b5c
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
msc.type: authoredcontent
ms.openlocfilehash: 08de8a57860886a8f633cacbaae1d63fe08a5a02
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752676"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-c"></a><span data-ttu-id="c2e0a-104">Tworzenie testów jednostkowych dla aplikacji ASP.NET MVC (C#)</span><span class="sxs-lookup"><span data-stu-id="c2e0a-104">Creating Unit Tests for ASP.NET MVC Applications (C#)</span></span>
====================
<span data-ttu-id="c2e0a-105">przez [Walther Autor: Stephen](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="c2e0a-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="c2e0a-106">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="c2e0a-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_CS.pdf)

> <span data-ttu-id="c2e0a-107">Informacje o sposobie tworzenia testów jednostkowych dla akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-107">Learn how to create unit tests for controller actions.</span></span> <span data-ttu-id="c2e0a-108">W tym samouczku Walther Autor: Stephen pokazuje, jak sprawdzić, czy akcji kontrolera zwraca określonego widoku, zwraca wartość określonego zestawu danych lub zwraca inny typ wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-108">In this tutorial, Stephen Walther demonstrates how to test whether a controller action returns a particular view, returns a particular set of data, or returns a different type of action result.</span></span>


<span data-ttu-id="c2e0a-109">Celem tego samouczka jest pokazują, jak pisać testy jednostkowe dla kontrolerów w Twojej aplikacji ASP.NET MVC aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-109">The goal of this tutorial is to demonstrate how you can write unit tests for the controllers in your ASP.NET MVC applications.</span></span> <span data-ttu-id="c2e0a-110">Będziemy omawiać sposób tworzenia trzech różnych rodzajów testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-110">We discuss how to build three different types of unit tests.</span></span> <span data-ttu-id="c2e0a-111">Dowiesz się, jak Podgląd zwróconej przez akcję kontrolera testów, jak dane widoku, zwrócone przez akcji kontrolera testów i jak sprawdzić, czy jedna akcja kontrolera przekieruje Cię do drugiej akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-111">You learn how to test the view returned by a controller action, how to test the View Data returned by a controller action, and how to test whether or not one controller action redirects you to a second controller action.</span></span>

## <a name="creating-the-controller-under-test"></a><span data-ttu-id="c2e0a-112">Tworzenie kontrolera w ramach testu</span><span class="sxs-lookup"><span data-stu-id="c2e0a-112">Creating the Controller under Test</span></span>

<span data-ttu-id="c2e0a-113">Zacznijmy od utworzenia kontrolera, które Chcieliśmy przeprowadzić test.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-113">Let's start by creating the controller that we intend to test.</span></span> <span data-ttu-id="c2e0a-114">Kontroler, o nazwie `ProductController`, znajduje się w ofercie 1.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-114">The controller, named the `ProductController`, is contained in Listing 1.</span></span>

<span data-ttu-id="c2e0a-115">**1 — Lista `ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="c2e0a-115">**Listing 1 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample1.cs)]

<span data-ttu-id="c2e0a-116">`ProductController` Zawiera dwie metody akcji, o nazwie `Index()` i `Details()`.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-116">The `ProductController` contains two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="c2e0a-117">Obie metody akcji zwracają widoku.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-117">Both action methods return a view.</span></span> <span data-ttu-id="c2e0a-118">Należy zauważyć, że `Details()` akcji akceptuje parametr o nazwie identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-118">Notice that the `Details()` action accepts a parameter named Id.</span></span>

## <a name="testing-the-view-returned-by-a-controller"></a><span data-ttu-id="c2e0a-119">Testowanie widoku zwrócony przez kontroler</span><span class="sxs-lookup"><span data-stu-id="c2e0a-119">Testing the View returned by a Controller</span></span>

<span data-ttu-id="c2e0a-120">Wyobraź sobie, że chcemy sprawdzić czy `ProductController` zwraca widok z prawej strony.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-120">Imagine that we want to test whether or not the `ProductController` returns the right view.</span></span> <span data-ttu-id="c2e0a-121">Chcemy upewnić się, że w przypadku `ProductController.Details()` akcja jest wywoływana, jest zwracany w widoku szczegółów.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-121">We want to make sure that when the `ProductController.Details()` action is invoked, the Details view is returned.</span></span> <span data-ttu-id="c2e0a-122">Klasy testowej w ofercie 2 zawiera testu jednostkowego do testowania widoku zwrócony przez `ProductController.Details()` akcji.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-122">The test class in Listing 2 contains a unit test for testing the view returned by the `ProductController.Details()` action.</span></span>

<span data-ttu-id="c2e0a-123">**2 — Lista `ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="c2e0a-123">**Listing 2 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample2.cs)]

<span data-ttu-id="c2e0a-124">Klasa w ofercie 2 obejmuje metody testowej, o nazwie `TestDetailsView()`.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-124">The class in Listing 2 includes a test method named `TestDetailsView()`.</span></span> <span data-ttu-id="c2e0a-125">Ta metoda zawiera trzy wiersze kodu.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-125">This method contains three lines of code.</span></span> <span data-ttu-id="c2e0a-126">Pierwszy wiersz kodu tworzy nowe wystąpienie klasy `ProductController` klasy.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-126">The first line of code creates a new instance of the `ProductController` class.</span></span> <span data-ttu-id="c2e0a-127">Drugi wiersz kodu wywołuje kontrolera `Details()` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-127">The second line of code invokes the controller's `Details()` action method.</span></span> <span data-ttu-id="c2e0a-128">Na koniec, ostatni wiersz sprawdza kod określa, czy widok jest zwracany przez `Details()` akcja jest widok szczegółów.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-128">Finally, the last line of code checks whether or not the view returned by the `Details()` action is the Details view.</span></span>

<span data-ttu-id="c2e0a-129">`ViewResult.ViewName` Właściwość reprezentuje nazwę widoku, który został zwrócony przez kontroler.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-129">The `ViewResult.ViewName` property represents the name of the view returned by a controller.</span></span> <span data-ttu-id="c2e0a-130">Jeden duży ostrzeżenie dotyczące testowania tej właściwości.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-130">One big warning about testing this property.</span></span> <span data-ttu-id="c2e0a-131">Istnieją dwa sposoby, że kontroler może zwrócić widok.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-131">There are two ways that a controller can return a view.</span></span> <span data-ttu-id="c2e0a-132">Kontroler może zwrócić jawnie widoku następująco:</span><span class="sxs-lookup"><span data-stu-id="c2e0a-132">A controller can explicitly return a view like this:</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample3.cs)]

<span data-ttu-id="c2e0a-133">Alternatywnie Nazwa widoku można wywnioskować na podstawie nazwę akcji kontrolera następująco:</span><span class="sxs-lookup"><span data-stu-id="c2e0a-133">Alternatively, the name of the view can be inferred from the name of the controller action like this:</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample4.cs)]

<span data-ttu-id="c2e0a-134">Ta akcja kontroler zwraca również wartość widoku o nazwie `Details`.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-134">This controller action also returns a view named `Details`.</span></span> <span data-ttu-id="c2e0a-135">Jednak nazwa widoku jest wnioskowany na podstawie nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-135">However, the name of the view is inferred from the action name.</span></span> <span data-ttu-id="c2e0a-136">Jeśli chcesz przetestować nazwy widoku, następnie jawnie wróć nazwy widoku z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-136">If you want to test the view name, then you must explicitly return the view name from the controller action.</span></span>

<span data-ttu-id="c2e0a-137">Możesz uruchomić test jednostki w ofercie 2, wprowadzając kombinacja klawiszy **Ctrl-R, A** lub przez kliknięcie przycisku **Uruchom wszystkie testy w rozwiązaniu** przycisku (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="c2e0a-137">You can run the unit test in Listing 2 by either entering the keyboard combination **Ctrl-R, A** or by clicking the **Run All Tests in Solution** button (see Figure 1).</span></span> <span data-ttu-id="c2e0a-138">Jeśli test zakończy się pomyślnie, zostanie wyświetlone okno wyników testu na rysunku 2.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-138">If the test passes, you'll see the Test Results window in Figure 2.</span></span>


<span data-ttu-id="c2e0a-139">[![Uruchom wszystkie testy w rozwiązaniu](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c2e0a-139">[![Run All Tests in Solution](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)</span></span>

<span data-ttu-id="c2e0a-140">**Rysunek 01**: Uruchom wszystkie testy w rozwiązaniu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c2e0a-140">**Figure 01**: Run All Tests in Solution ([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))</span></span>


<span data-ttu-id="c2e0a-141">[![SUKCES!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="c2e0a-141">[![Success!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)</span></span>

<span data-ttu-id="c2e0a-142">**Rysunek 02**: Sukces!</span><span class="sxs-lookup"><span data-stu-id="c2e0a-142">**Figure 02**: Success!</span></span> <span data-ttu-id="c2e0a-143">([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="c2e0a-143">([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))</span></span>


## <a name="testing-the-view-data-returned-by-a-controller"></a><span data-ttu-id="c2e0a-144">Testowanie dane zwracane przez kontroler</span><span class="sxs-lookup"><span data-stu-id="c2e0a-144">Testing the View Data returned by a Controller</span></span>

<span data-ttu-id="c2e0a-145">Kontroler MVC przekazuje dane do widoku przy użyciu coś, co jest nazywane *`View Data`*.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-145">An MVC controller passes data to a view by using something called *`View Data`*.</span></span> <span data-ttu-id="c2e0a-146">Załóżmy, że chcesz wyświetlić szczegółowe informacje dotyczące konkretnego produktu po wywołaniu `ProductController Details()` akcji.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-146">For example, imagine that you want to display the details for a particular product when you invoke the `ProductController Details()` action.</span></span> <span data-ttu-id="c2e0a-147">W takim przypadku można utworzyć wystąpienia `Product` klasy (zdefiniowanymi w modelu) i przekaż wystąpienie `Details` widoku, wykorzystując `View Data`.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-147">In that case, you can create an instance of a `Product` class (defined in your model) and pass the instance to the `Details` view by taking advantage of `View Data`.</span></span>

<span data-ttu-id="c2e0a-148">Zmodyfikowanego `ProductController` w ofercie 3 zawiera zaktualizowanego `Details()` akcję, która zwraca produktu.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-148">The modified `ProductController` in Listing 3 includes an updated `Details()` action that returns a Product.</span></span>

<span data-ttu-id="c2e0a-149">**3 — lista `ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="c2e0a-149">**Listing 3 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample5.cs)]

<span data-ttu-id="c2e0a-150">Po pierwsze, `Details()` akcja tworzy nowe wystąpienie klasy `Product` klasa, która reprezentuje komputera przenośnego.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-150">First, the `Details()` action creates a new instance of the `Product` class that represents a laptop computer.</span></span> <span data-ttu-id="c2e0a-151">Następnie, wystąpienie `Product` klasy jest przekazywany jako drugi parametr `View()` metody.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-151">Next, the instance of the `Product` class is passed as the second parameter to the `View()` method.</span></span>

<span data-ttu-id="c2e0a-152">Można pisać testy jednostkowe do sprawdzenia, czy oczekiwanych danych znajdujących się w widoku danych.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-152">You can write unit tests to test whether the expected data is contained in view data.</span></span> <span data-ttu-id="c2e0a-153">Testów jednostkowych w testach listę 4, czy produkt reprezentujące komputer przenośny jest jest zwracany, jeśli wywołujesz `ProductController Details()` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-153">The unit test in Listing 4 tests whether or not a Product representing a laptop computer is returned when you call the `ProductController Details()` action method.</span></span>

<span data-ttu-id="c2e0a-154">**4 — lista `ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="c2e0a-154">**Listing 4 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample6.cs)]

<span data-ttu-id="c2e0a-155">W ofercie 4 `TestDetailsView()` metoda sprawdza dane widoku, zwracany przez wywołanie `Details()` metody.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-155">In Listing 4, the `TestDetailsView()` method tests the View Data returned by invoking the `Details()` method.</span></span> <span data-ttu-id="c2e0a-156">`ViewData` Jest uwidaczniany jako właściwość w `ViewResult` zwracany przez wywołanie `Details()` metody.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-156">The `ViewData` is exposed as a property on the `ViewResult` returned by invoking the `Details()` method.</span></span> <span data-ttu-id="c2e0a-157">`ViewData.Model` Właściwość zawiera produktu przekazywane do widoku.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-157">The `ViewData.Model` property contains the product passed to the view.</span></span> <span data-ttu-id="c2e0a-158">Po prostu ten test sprawdza, czy produktu zawarty w widoku danych ma nazwę komputera przenośnego.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-158">The test simply verifies that the product contained in the View Data has the name Laptop.</span></span>

## <a name="testing-the-action-result-returned-by-a-controller"></a><span data-ttu-id="c2e0a-159">Testując otrzymany wynik akcji zwrócony przez kontroler</span><span class="sxs-lookup"><span data-stu-id="c2e0a-159">Testing the Action Result returned by a Controller</span></span>

<span data-ttu-id="c2e0a-160">Bardziej złożone akcji kontrolera może być zwracanie różnych typów wyników akcji w zależności od wartości parametrów przekazanych do akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-160">A more complex controller action might return different types of action results depending on the values of the parameters passed to the controller action.</span></span> <span data-ttu-id="c2e0a-161">Akcja kontrolera może zwracać różne typy wyników akcji, w tym `ViewResult`, `RedirectToRouteResult`, lub `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-161">A controller action can return a variety of types of action results including a `ViewResult`, `RedirectToRouteResult`, or `JsonResult`.</span></span>

<span data-ttu-id="c2e0a-162">Na przykład zmodyfikowane `Details()` zwraca akcji w ofercie 5 `Details` widoku, gdy przekazujesz prawidłowy identyfikator produktu do akcji.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-162">For example, the modified `Details()` action in Listing 5 returns the `Details` view when you pass a valid product Id to the action.</span></span> <span data-ttu-id="c2e0a-163">W przypadku przekazania nieprawidłowy identyfikator — identyfikator produktu o wartości, mniej niż 1 — a następnie zostanie przekierowany do `Index()` akcji.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-163">If you pass an invalid product Id -- an Id with a value less than 1 -- then you are redirected to the `Index()` action.</span></span>

<span data-ttu-id="c2e0a-164">**5 — lista `ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="c2e0a-164">**Listing 5 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample7.cs)]

<span data-ttu-id="c2e0a-165">Możesz przetestować działanie `Details()` akcji z testu jednostkowego w ofercie 6.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-165">You can test the behavior of the `Details()` action with the unit test in Listing 6.</span></span> <span data-ttu-id="c2e0a-166">Test jednostkowy w ofercie 6 sprawdza, czy nastąpi przekierowanie do `Index` widoku, gdy identyfikatora o wartości -1 jest przekazywany do `Details()` metody.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-166">The unit test in Listing 6 verifies that you are redirected to the `Index` view when an Id with the value -1 is passed to the `Details()` method.</span></span>

<span data-ttu-id="c2e0a-167">**6 — lista `ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="c2e0a-167">**Listing 6 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample8.cs)]

<span data-ttu-id="c2e0a-168">Gdy wywołujesz `RedirectToAction()` zwraca akcji kontrolera metody akcji kontrolera, `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-168">When you call the `RedirectToAction()` method in a controller action, the controller action returns a `RedirectToRouteResult`.</span></span> <span data-ttu-id="c2e0a-169">Sprawdzanie badania czy `RedirectToRouteResult` spowoduje przekierowanie użytkownika do akcji kontrolera, o nazwie `Index`.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-169">The test checks whether the `RedirectToRouteResult` will redirect the user to a controller action named `Index`.</span></span>

## <a name="summary"></a><span data-ttu-id="c2e0a-170">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="c2e0a-170">Summary</span></span>

<span data-ttu-id="c2e0a-171">W tym samouczku przedstawiono sposób tworzenia testów jednostkowych dla akcji kontrolera MVC.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-171">In this tutorial, you learned how to build unit tests for MVC controller actions.</span></span> <span data-ttu-id="c2e0a-172">Po pierwsze pokazaliśmy ci, jak sprawdzić, czy widok z prawej strony jest zwracany przez akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-172">First, you learned how to verify whether the right view is returned by a controller action.</span></span> <span data-ttu-id="c2e0a-173">Przedstawiono sposób użycia `ViewResult.ViewName` właściwości, aby sprawdzić nazwę widoku.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-173">You learned how to use the `ViewResult.ViewName` property to verify the name of a view.</span></span>

<span data-ttu-id="c2e0a-174">Następnie zbadaliśmy, jak można sprawdzić zawartość `View Data`.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-174">Next, we examined how you can test the contents of `View Data`.</span></span> <span data-ttu-id="c2e0a-175">Pokazaliśmy ci, jak sprawdzić, czy prawy produkt został zwrócony w `View Data` po wywołaniu akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-175">You learned how to check whether the right product was returned in `View Data` after calling a controller action.</span></span>

<span data-ttu-id="c2e0a-176">Na koniec omówiono, jak można sprawdzić, czy zwracane są różnych typów wyników akcji z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-176">Finally, we discussed how you can test whether different types of action results are returned from a controller action.</span></span> <span data-ttu-id="c2e0a-177">Przedstawiono sposób sprawdzić, czy kontroler zwraca `ViewResult` lub `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="c2e0a-177">You learned how to test whether a controller returns a `ViewResult` or a `RedirectToRouteResult`.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c2e0a-178">Next</span><span class="sxs-lookup"><span data-stu-id="c2e0a-178">Next</span></span>](creating-unit-tests-for-asp-net-mvc-applications-vb.md)
