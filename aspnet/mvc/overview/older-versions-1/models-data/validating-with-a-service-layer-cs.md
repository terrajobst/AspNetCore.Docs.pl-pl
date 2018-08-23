---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Weryfikowanie z użyciem warstwy usług (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Dowiedz się, jak przenieść swoją logikę weryfikacji poza akcji kontrolera, a także do oddzielnej usługi warstwy. W tym samouczku wyjaśniono, Autor: Stephen Walther jak możesz...'
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 69ff78949589017d12a791231e38b400b49f2917
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752178"
---
<a name="validating-with-a-service-layer-c"></a><span data-ttu-id="00cc5-104">Weryfikowanie z użyciem warstwy usług (C#)</span><span class="sxs-lookup"><span data-stu-id="00cc5-104">Validating with a Service Layer (C#)</span></span>
====================
<span data-ttu-id="00cc5-105">przez [Walther Autor: Stephen](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="00cc5-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="00cc5-106">Dowiedz się, jak przenieść swoją logikę weryfikacji poza akcji kontrolera, a także do oddzielnej usługi warstwy.</span><span class="sxs-lookup"><span data-stu-id="00cc5-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="00cc5-107">W tym samouczku Walther Autor: Stephen wyjaśnia, jak możesz zachować sharp separacji zagadnień przez izolowanie swojej warstwy usług z warstwą kontrolera.</span><span class="sxs-lookup"><span data-stu-id="00cc5-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="00cc5-108">Celem tego samouczka jest opisują jedną z metod wykonywania weryfikacji w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="00cc5-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="00cc5-109">W tym samouczku dowiesz się, jak przenieść swoją logikę weryfikacji poza kontrolerach i do oddzielnej usługi warstwy.</span><span class="sxs-lookup"><span data-stu-id="00cc5-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="00cc5-110">Oddzielenie obaw</span><span class="sxs-lookup"><span data-stu-id="00cc5-110">Separating Concerns</span></span>

<span data-ttu-id="00cc5-111">W przypadku tworzenia aplikacji ASP.NET MVC, nie należy umieszczać logikę bazy danych wewnątrz akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="00cc5-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="00cc5-112">Mieszanie logikę bazy danych i kontroler utrudnia aplikacji do obsługi wraz z upływem czasu.</span><span class="sxs-lookup"><span data-stu-id="00cc5-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="00cc5-113">Zaleca się umieścić wszystkie logiki bazy danych w warstwie osobnym repozytorium.</span><span class="sxs-lookup"><span data-stu-id="00cc5-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="00cc5-114">Na przykład wyświetlanie listy 1 zawiera prostego repozytorium o nazwie ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="00cc5-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="00cc5-115">Repozytorium produktu zawiera wszystkie kod dostępu do danych dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00cc5-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="00cc5-116">Listę obejmuje również interfejs IProductRepository, który implementuje repozytorium produktu.</span><span class="sxs-lookup"><span data-stu-id="00cc5-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="00cc5-117">**Wyświetlanie listy 1 — Models\ProductRepository.cs**</span><span class="sxs-lookup"><span data-stu-id="00cc5-117">**Listing 1 -- Models\ProductRepository.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

<span data-ttu-id="00cc5-118">Kontroler w ofercie 2 używa warstwy repozytorium w jego indeks() i Create() akcji.</span><span class="sxs-lookup"><span data-stu-id="00cc5-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="00cc5-119">Zwróć uwagę, czy ten kontroler nie zawiera wszelka logika bazy danych.</span><span class="sxs-lookup"><span data-stu-id="00cc5-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="00cc5-120">Tworzenie warstwy repozytorium pozwala zachować czyste rozdzielenie problemów.</span><span class="sxs-lookup"><span data-stu-id="00cc5-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="00cc5-121">Kontrolery są odpowiedzialne za logiki kontroli przepływu aplikacji i repozytorium jest odpowiedzialny za logiką dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="00cc5-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="00cc5-122">**Wyświetlanie listy 2 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="00cc5-122">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="00cc5-123">Tworzenie warstwy usług</span><span class="sxs-lookup"><span data-stu-id="00cc5-123">Creating a Service Layer</span></span>

<span data-ttu-id="00cc5-124">Tak logiki kontroli przepływu w aplikacji należy w kontrolerze i logiką dostępu do danych, należy w repozytorium.</span><span class="sxs-lookup"><span data-stu-id="00cc5-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="00cc5-125">W takiej sytuacji, gdzie należy umieścić logikę weryfikacji?</span><span class="sxs-lookup"><span data-stu-id="00cc5-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="00cc5-126">Jedną z opcji jest umieszczenie logikę weryfikacji w *warstwy usług*.</span><span class="sxs-lookup"><span data-stu-id="00cc5-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="00cc5-127">Warstwy usług to dodatkowa warstwa w aplikacji ASP.NET MVC, która pośredniczy komunikacji między kontrolerem i warstwy repozytorium.</span><span class="sxs-lookup"><span data-stu-id="00cc5-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="00cc5-128">Warstwy usługi zawiera logikę biznesową.</span><span class="sxs-lookup"><span data-stu-id="00cc5-128">The service layer contains business logic.</span></span> <span data-ttu-id="00cc5-129">W szczególności zawiera logikę weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="00cc5-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="00cc5-130">Na przykład warstwy usług produktu w ofercie 3 ma metodę CreateProduct().</span><span class="sxs-lookup"><span data-stu-id="00cc5-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="00cc5-131">Metoda CreateProduct() wywołuje metodę ValidateProduct() do sprawdzania poprawności nowego produktu przed przekazaniem produktu do repozytorium produktu.</span><span class="sxs-lookup"><span data-stu-id="00cc5-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="00cc5-132">**Wyświetlanie listy 3 - Models\ProductService.cs**</span><span class="sxs-lookup"><span data-stu-id="00cc5-132">**Listing 3 - Models\ProductService.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

<span data-ttu-id="00cc5-133">Kontroler produktu została zaktualizowana w ofercie 4, aby używać warstwy usług zamiast warstwy repozytorium.</span><span class="sxs-lookup"><span data-stu-id="00cc5-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="00cc5-134">Warstwa kontrolera rozmawia z warstwy usług.</span><span class="sxs-lookup"><span data-stu-id="00cc5-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="00cc5-135">Warstwy usług rozmawia z warstwy repozytorium.</span><span class="sxs-lookup"><span data-stu-id="00cc5-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="00cc5-136">Każda warstwa ma oddzielne odpowiedzialności.</span><span class="sxs-lookup"><span data-stu-id="00cc5-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="00cc5-137">**Wyświetlanie listy 4 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="00cc5-137">**Listing 4 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

<span data-ttu-id="00cc5-138">Należy zauważyć, że usługa produktu jest tworzona w Konstruktorze kontroler produktu.</span><span class="sxs-lookup"><span data-stu-id="00cc5-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="00cc5-139">Po utworzeniu usługi produktów słownik stanów modelu jest przekazywany do usługi.</span><span class="sxs-lookup"><span data-stu-id="00cc5-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="00cc5-140">Usługa produktu używa stan modelu do przekazywania komunikatów o błędach weryfikacji odsyłane do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="00cc5-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="00cc5-141">Oddzielenie warstwy usług</span><span class="sxs-lookup"><span data-stu-id="00cc5-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="00cc5-142">Firma Microsoft nie powiodło się izolować kontrolera i warstwy usługi, w związku z jednego.</span><span class="sxs-lookup"><span data-stu-id="00cc5-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="00cc5-143">Kontroler i warstwy usługi komunikują się za pośrednictwem stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="00cc5-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="00cc5-144">Innymi słowy warstwy usług zależny od określonej funkcji platformę ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="00cc5-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="00cc5-145">Chcemy wyizolować warstwy usług z naszych możliwie warstwy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="00cc5-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="00cc5-146">Teoretycznie możemy powinno być możliwe do warstwy usług za pomocą dowolnego typu aplikacji i nie tylko aplikację ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="00cc5-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="00cc5-147">Na przykład w przyszłości może być chcemy tworzenie aplikacji WPF frontonu dla naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00cc5-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="00cc5-148">Należy znaleźliśmy sposobem usunięcia zależności na platformie ASP.NET MVC stan modelu z naszych warstwy usług.</span><span class="sxs-lookup"><span data-stu-id="00cc5-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="00cc5-149">W ofercie 5 warstwy usług został zaktualizowany tak, aby nie używał już stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="00cc5-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="00cc5-150">Zamiast tego używa każdej klasy, która implementuje interfejs IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="00cc5-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="00cc5-151">**Wyświetlanie listy 5 - Models\ProductService.cs (odłączone)**</span><span class="sxs-lookup"><span data-stu-id="00cc5-151">**Listing 5 - Models\ProductService.cs (decoupled)**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

<span data-ttu-id="00cc5-152">Interfejs IValidationDictionary jest zdefiniowany w ofercie 6.</span><span class="sxs-lookup"><span data-stu-id="00cc5-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="00cc5-153">Ten prosty interfejs zawiera jedną metodę i jedną właściwość.</span><span class="sxs-lookup"><span data-stu-id="00cc5-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="00cc5-154">**Wyświetlanie listy 6 - Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="00cc5-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

<span data-ttu-id="00cc5-155">Klasa w ofercie 7 o nazwie klasy ModelStateWrapper implementuje interfejs IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="00cc5-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="00cc5-156">Można utworzyć wystąpienie klasy ModelStateWrapper, przekazując słownik stanów modelu do konstruktora.</span><span class="sxs-lookup"><span data-stu-id="00cc5-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="00cc5-157">**Wyświetlanie listy 7 - Models\ModelStateWrapper.cs**</span><span class="sxs-lookup"><span data-stu-id="00cc5-157">**Listing 7 - Models\ModelStateWrapper.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

<span data-ttu-id="00cc5-158">Ponadto zaktualizowano kontroler w ofercie 8 używa ModelStateWrapper podczas tworzenia warstwy usług w jej konstruktora.</span><span class="sxs-lookup"><span data-stu-id="00cc5-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="00cc5-159">**Wyświetlanie listy 8 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="00cc5-159">**Listing 8 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

<span data-ttu-id="00cc5-160">Za pomocą IValidationDictionary interfejsu i klasa ModelStateWrapper pozwala nam całkowicie odizolować nasze warstwy usług z naszych warstwy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="00cc5-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="00cc5-161">Warstwy usług nie jest już zależny od stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="00cc5-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="00cc5-162">Można przekazać każdej klasy, która implementuje interfejs IValidationDictionary do warstwy usług.</span><span class="sxs-lookup"><span data-stu-id="00cc5-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="00cc5-163">Na przykład aplikacja WPF może implementować interfejs IValidationDictionary klasa prostych kolekcji.</span><span class="sxs-lookup"><span data-stu-id="00cc5-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="00cc5-164">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="00cc5-164">Summary</span></span>

<span data-ttu-id="00cc5-165">Celem tego samouczka było w celu omówienia jedno z podejść do przeprowadzania weryfikacji w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="00cc5-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="00cc5-166">W tym samouczku przedstawiono sposób przenieść całą logikę weryfikacji poza kontrolerach i do oddzielnej usługi warstwy.</span><span class="sxs-lookup"><span data-stu-id="00cc5-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="00cc5-167">Przedstawiono również sposób izolowania swojej warstwy usług z warstwą kontrolera, tworząc klasę ModelStateWrapper.</span><span class="sxs-lookup"><span data-stu-id="00cc5-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="00cc5-168">[Poprzednie](validating-with-the-idataerrorinfo-interface-cs.md)
> [dalej](validation-with-the-data-annotation-validators-cs.md)</span><span class="sxs-lookup"><span data-stu-id="00cc5-168">[Previous](validating-with-the-idataerrorinfo-interface-cs.md)
[Next](validation-with-the-data-annotation-validators-cs.md)</span></span>
