---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: "Sprawdzanie poprawności z warstwy usług (VB) | Dokumentacja firmy Microsoft"
author: StephenWalther
description: "Dowiedz się, jak przenieść logiki sprawdzania poprawności poza akcji kontrolera i do warstwy oddzielne usługi. W tym samouczku Stephen Walther wyjaśniono, jak można..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 5a8f1dd888c7fa6a3353b7b748a0ffa30b94149c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="validating-with-a-service-layer-vb"></a><span data-ttu-id="00bc3-104">Sprawdzanie poprawności z warstwy usług (VB)</span><span class="sxs-lookup"><span data-stu-id="00bc3-104">Validating with a Service Layer (VB)</span></span>
====================
<span data-ttu-id="00bc3-105">przez [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="00bc3-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="00bc3-106">Dowiedz się, jak przenieść logiki sprawdzania poprawności poza akcji kontrolera i do warstwy oddzielne usługi.</span><span class="sxs-lookup"><span data-stu-id="00bc3-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="00bc3-107">W tym samouczku Stephen Walther wyjaśniono, jak można zachować sharp separacji przez izolowanie z warstwy usług z warstwą kontrolera.</span><span class="sxs-lookup"><span data-stu-id="00bc3-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="00bc3-108">Celem tego samouczka jest do opisywania jedną metodę przeprowadzania weryfikacji w aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="00bc3-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="00bc3-109">W tym samouczku Dowiedz się jak przenieść logiki sprawdzania poprawności z kontrolerami poza i do warstwy oddzielne usługi.</span><span class="sxs-lookup"><span data-stu-id="00bc3-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="00bc3-110">Oddzielanie problemy</span><span class="sxs-lookup"><span data-stu-id="00bc3-110">Separating Concerns</span></span>

<span data-ttu-id="00bc3-111">Podczas tworzenia aplikacji platformy ASP.NET MVC, nie należy umieszczać logiki bazy danych wewnątrz akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="00bc3-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="00bc3-112">Mieszanie logiki bazy danych i kontrolera utrudnia aplikacji do obsługi wraz z upływem czasu.</span><span class="sxs-lookup"><span data-stu-id="00bc3-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="00bc3-113">Zaleca się umieścić wszystkie logiki bazy danych w warstwie oddzielne repozytorium.</span><span class="sxs-lookup"><span data-stu-id="00bc3-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="00bc3-114">Na przykład 1 Lista zawiera prostego repozytorium o nazwie ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="00bc3-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="00bc3-115">Repozytorium produktu zawiera wszystkie dane kod dostępu dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00bc3-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="00bc3-116">Lista zawiera również interfejs IProductRepository, który implementuje repozytorium produktu.</span><span class="sxs-lookup"><span data-stu-id="00bc3-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="00bc3-117">**Wyświetlanie listy 1 - Models\ProductRepository.vb**</span><span class="sxs-lookup"><span data-stu-id="00bc3-117">**Listing 1 - Models\ProductRepository.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

<span data-ttu-id="00bc3-118">Kontroler wyświetlania 2 używa warstwy repozytorium zarówno jego indeks() i Create() akcje.</span><span class="sxs-lookup"><span data-stu-id="00bc3-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="00bc3-119">Zwróć uwagę, czy ten kontroler nie zawiera wszelka logika bazy danych.</span><span class="sxs-lookup"><span data-stu-id="00bc3-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="00bc3-120">Tworzenie warstwy repozytorium umożliwia zachowanie separacji czyste.</span><span class="sxs-lookup"><span data-stu-id="00bc3-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="00bc3-121">Kontrolery są odpowiedzialne za logiki kontroli przepływu aplikacji i repozytorium jest odpowiedzialny za logika dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="00bc3-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="00bc3-122">**Wyświetlanie listy 2 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="00bc3-122">**Listing 2 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="00bc3-123">Tworzenie warstwy usług</span><span class="sxs-lookup"><span data-stu-id="00bc3-123">Creating a Service Layer</span></span>

<span data-ttu-id="00bc3-124">Tak logiki kontroli przepływu aplikacji należy w kontrolerze i logika dostępu do danych, należy w repozytorium.</span><span class="sxs-lookup"><span data-stu-id="00bc3-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="00bc3-125">W takim przypadku, gdy opracować logiki sprawdzania poprawności?</span><span class="sxs-lookup"><span data-stu-id="00bc3-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="00bc3-126">Jedną z opcji jest umieszczenie logiki sprawdzania poprawności w *warstwy usług*.</span><span class="sxs-lookup"><span data-stu-id="00bc3-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="00bc3-127">Warstwy usług to dodatkowa warstwa w aplikacji ASP.NET MVC, która przekazuje komunikację między kontrolerem i warstwy repozytorium.</span><span class="sxs-lookup"><span data-stu-id="00bc3-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="00bc3-128">Warstwy usługi zawiera logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="00bc3-128">The service layer contains business logic.</span></span> <span data-ttu-id="00bc3-129">W szczególności zawiera logikę weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="00bc3-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="00bc3-130">Na przykład warstwy usług produktu w wersji 3 wyświetlania ma metodę CreateProduct().</span><span class="sxs-lookup"><span data-stu-id="00bc3-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="00bc3-131">Metoda CreateProduct() wywołuje metodę ValidateProduct() do sprawdzania poprawności nowego produktu przed przekazaniem produktu do repozytorium produktu.</span><span class="sxs-lookup"><span data-stu-id="00bc3-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="00bc3-132">**Wyświetlanie listy 3 - Models\ProductService.vb**</span><span class="sxs-lookup"><span data-stu-id="00bc3-132">**Listing 3 - Models\ProductService.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

<span data-ttu-id="00bc3-133">Kontroler produktu został zaktualizowany w listę 4, aby używać warstwy usług zamiast warstwy repozytorium.</span><span class="sxs-lookup"><span data-stu-id="00bc3-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="00bc3-134">Warstwa kontrolera komunikuje się warstwy usług.</span><span class="sxs-lookup"><span data-stu-id="00bc3-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="00bc3-135">Warstwy usług komunikuje się z warstwą repozytorium.</span><span class="sxs-lookup"><span data-stu-id="00bc3-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="00bc3-136">Każda warstwa ma oddzielne odpowiedzialności.</span><span class="sxs-lookup"><span data-stu-id="00bc3-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="00bc3-137">**Wyświetlanie listy 4 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="00bc3-137">**Listing 4 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

<span data-ttu-id="00bc3-138">Zwróć uwagę, czy usługa produktu został utworzony w Konstruktorze kontroler produktu.</span><span class="sxs-lookup"><span data-stu-id="00bc3-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="00bc3-139">Po utworzeniu usługi produktów słownik stanów modelu jest przekazywany do usługi.</span><span class="sxs-lookup"><span data-stu-id="00bc3-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="00bc3-140">Usługa produktu używa stan modelu do przekazywania komunikatów o błędach weryfikacji do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="00bc3-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="00bc3-141">Rozdzielenie warstwy usług</span><span class="sxs-lookup"><span data-stu-id="00bc3-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="00bc3-142">Firma Microsoft nie izolować kontrolera i warstwy usługi, w odniesieniu do jednego.</span><span class="sxs-lookup"><span data-stu-id="00bc3-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="00bc3-143">Kontroler i warstwy usługi do komunikacji za pośrednictwem stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="00bc3-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="00bc3-144">Innymi słowy warstwy usług ma zależność na poszczególnych funkcji platformę ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="00bc3-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="00bc3-145">Chcemy warstwy usług z naszych możliwie warstwy kontrolera Izoluj.</span><span class="sxs-lookup"><span data-stu-id="00bc3-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="00bc3-146">Teoretycznie możemy należy używać warstwy usług z dowolnego typu aplikacji i nie tylko aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="00bc3-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="00bc3-147">Na przykład w przyszłości może chcemy kompilacji frontonu dla aplikacji WPF.</span><span class="sxs-lookup"><span data-stu-id="00bc3-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="00bc3-148">Firma Microsoft stwierdzi, sposób, aby usunąć zależności na platformie ASP.NET MVC stan modelu z naszych warstwy usług.</span><span class="sxs-lookup"><span data-stu-id="00bc3-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="00bc3-149">Wyświetlanie listy 5 warstwy usług został zaktualizowany tak, aby nie używał już stan modelu.</span><span class="sxs-lookup"><span data-stu-id="00bc3-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="00bc3-150">Zamiast tego używa dowolnej klasy, która implementuje interfejs IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="00bc3-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="00bc3-151">**Wyświetlanie listy 5 - Models\ProductService.vb (całkowicie niezależna)**</span><span class="sxs-lookup"><span data-stu-id="00bc3-151">**Listing 5 - Models\ProductService.vb (decoupled)**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

<span data-ttu-id="00bc3-152">Interfejs IValidationDictionary jest zdefiniowany w 6 wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="00bc3-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="00bc3-153">Ten prosty interfejs ma jedną metodę i jednej właściwości.</span><span class="sxs-lookup"><span data-stu-id="00bc3-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="00bc3-154">**Wyświetlanie listy 6 - Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="00bc3-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

<span data-ttu-id="00bc3-155">Klasa w 7 wyświetlania o nazwie klasy ModelStateWrapper implementuje interfejs IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="00bc3-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="00bc3-156">Można utworzyć wystąpienia klasy ModelStateWrapper przez przekazanie słownik stanów modelu do konstruktora.</span><span class="sxs-lookup"><span data-stu-id="00bc3-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="00bc3-157">**Wyświetlanie listy 7 - Models\ModelStateWrapper.vb**</span><span class="sxs-lookup"><span data-stu-id="00bc3-157">**Listing 7 - Models\ModelStateWrapper.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

<span data-ttu-id="00bc3-158">Ponadto zaktualizowane kontrolera w wyświetlania 8 używa ModelStateWrapper podczas tworzenia warstwy usług w jego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="00bc3-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="00bc3-159">**Wyświetlanie listy 8 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="00bc3-159">**Listing 8 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

<span data-ttu-id="00bc3-160">Używanie IValidationDictionary interfejsu i klasa ModelStateWrapper umożliwia całkowicie rozdzielanie naszych warstwy usług z naszych warstwy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="00bc3-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="00bc3-161">Warstwy usług nie jest już zależny od stanu modelu.</span><span class="sxs-lookup"><span data-stu-id="00bc3-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="00bc3-162">Można przekazać każda klasa implementująca interfejs IValidationDictionary do warstwy usług.</span><span class="sxs-lookup"><span data-stu-id="00bc3-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="00bc3-163">Na przykład w aplikacji WPF mogą implementować interfejs IValidationDictionary z klasa prostych kolekcji.</span><span class="sxs-lookup"><span data-stu-id="00bc3-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="00bc3-164">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="00bc3-164">Summary</span></span>

<span data-ttu-id="00bc3-165">Celem tego samouczka został omówimy jeden ze sposobów przeprowadzania weryfikacji w aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="00bc3-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="00bc3-166">W tym samouczku przedstawiono sposób Przenieś wszystkie logiki sprawdzania poprawności z kontrolerami poza i do warstwy oddzielne usługi.</span><span class="sxs-lookup"><span data-stu-id="00bc3-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="00bc3-167">Przedstawiono również sposób wyizolować z warstwy usług z warstwą kontrolera, tworząc klasę ModelStateWrapper.</span><span class="sxs-lookup"><span data-stu-id="00bc3-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="00bc3-168">[Poprzednie](validating-with-the-idataerrorinfo-interface-vb.md)
[dalej](validation-with-the-data-annotation-validators-vb.md)</span><span class="sxs-lookup"><span data-stu-id="00bc3-168">[Previous](validating-with-the-idataerrorinfo-interface-vb.md)
[Next](validation-with-the-data-annotation-validators-vb.md)</span></span>
