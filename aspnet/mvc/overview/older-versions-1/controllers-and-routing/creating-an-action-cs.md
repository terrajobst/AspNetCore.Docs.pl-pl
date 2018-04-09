---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Tworzenie akcji (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Dowiedz się, jak dodać nową akcję do kontrolera ASP.NET MVC. Dowiedz się więcej o wymaganiach dotyczących metodę, aby akcji.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c6145902db59b07e96a5563b138c1a6323946b2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-an-action-c"></a><span data-ttu-id="47d9b-104">Tworzenie akcji (C#)</span><span class="sxs-lookup"><span data-stu-id="47d9b-104">Creating an Action (C#)</span></span>
====================
<span data-ttu-id="47d9b-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="47d9b-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="47d9b-106">Dowiedz się, jak dodać nową akcję do kontrolera ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="47d9b-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="47d9b-107">Dowiedz się więcej o wymaganiach dotyczących metodę, aby akcji.</span><span class="sxs-lookup"><span data-stu-id="47d9b-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="47d9b-108">Celem tego samouczka jest opisano sposób tworzenia nowej akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="47d9b-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="47d9b-109">Poznaj wymagania dotyczące metody akcji.</span><span class="sxs-lookup"><span data-stu-id="47d9b-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="47d9b-110">Możesz również sposób zapobiec przypadkowym jako akcja metody.</span><span class="sxs-lookup"><span data-stu-id="47d9b-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="47d9b-111">Dodawanie akcji do kontrolera</span><span class="sxs-lookup"><span data-stu-id="47d9b-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="47d9b-112">Możesz dodać nową akcję do kontrolera, dodając nową metodę do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="47d9b-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="47d9b-113">Na przykład kontrolera 1 Lista zawiera akcji o nazwie indeks() i o nazwie SayHello().</span><span class="sxs-lookup"><span data-stu-id="47d9b-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="47d9b-114">Obie metody są widoczne jako akcje.</span><span class="sxs-lookup"><span data-stu-id="47d9b-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="47d9b-115">**Wyświetlanie listy 1 - Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="47d9b-115">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

<span data-ttu-id="47d9b-116">Aby udostępnić universe jako akcja, metody muszą spełniać określone wymagania:</span><span class="sxs-lookup"><span data-stu-id="47d9b-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="47d9b-117">Metoda musi być publiczny.</span><span class="sxs-lookup"><span data-stu-id="47d9b-117">The method must be public.</span></span>
- <span data-ttu-id="47d9b-118">Metoda nie może być metodą statyczną.</span><span class="sxs-lookup"><span data-stu-id="47d9b-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="47d9b-119">Metoda nie może być metodą rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="47d9b-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="47d9b-120">Metoda nie może być Konstruktor, metody pobierającej lub ustawiającej.</span><span class="sxs-lookup"><span data-stu-id="47d9b-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="47d9b-121">Metoda nie może mieć Otwórz typów ogólnych.</span><span class="sxs-lookup"><span data-stu-id="47d9b-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="47d9b-122">Metoda nie jest metodą klasy podstawowej kontrolera.</span><span class="sxs-lookup"><span data-stu-id="47d9b-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="47d9b-123">Metoda nie może zawierać **ref** lub **limit** parametrów.</span><span class="sxs-lookup"><span data-stu-id="47d9b-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="47d9b-124">Zwróć uwagę, że nie ma żadnych ograniczeń na typ zwrotny akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="47d9b-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="47d9b-125">Akcja kontrolera może zwrócić typu string, DateTime, wystąpienie klasy Random lub void.</span><span class="sxs-lookup"><span data-stu-id="47d9b-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="47d9b-126">Platforma ASP.NET MVC zostanie przekonwertować zwracany typ, który nie jest wynik akcji na ciąg i renderowania ciąg do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="47d9b-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="47d9b-127">Po dodaniu dowolnej metody, która narusza wymagania do kontrolera metody jest ujawniona jako akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="47d9b-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="47d9b-128">Należy zachować ostrożność, w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="47d9b-128">Be careful here.</span></span> <span data-ttu-id="47d9b-129">Akcja kontrolera może być wywoływany przez każdy komputer połączony z Internetem.</span><span class="sxs-lookup"><span data-stu-id="47d9b-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="47d9b-130">Nie, na przykład utworzyć DeleteMyWebsite() akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="47d9b-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="47d9b-131">Uniemożliwia publicznego — metoda</span><span class="sxs-lookup"><span data-stu-id="47d9b-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="47d9b-132">Jeśli musisz utworzyć publiczną metodę w klasie kontrolera i nie chcesz ujawnia metody akcji kontrolera może uniemożliwić metody wywoływanie przy użyciu atrybutu [NonAction].</span><span class="sxs-lookup"><span data-stu-id="47d9b-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the [NonAction] attribute.</span></span> <span data-ttu-id="47d9b-133">Na przykład kontroler wyświetlania 2 zawiera metody publicznej o nazwie CompanySecrets(), który zostanie nadany atrybut [NonAction].</span><span class="sxs-lookup"><span data-stu-id="47d9b-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the [NonAction] attribute.</span></span>

<span data-ttu-id="47d9b-134">**Wyświetlanie listy 2 - Controllers\WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="47d9b-134">**Listing 2 - Controllers\WorkController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

<span data-ttu-id="47d9b-135">Jeśli będziesz próbować wywołać CompanySecrets() akcji kontrolera, wpisując /Work/CompanySecrets na pasku adresu przeglądarki następnie zostanie wyświetlony komunikat o błędzie na rysunku 1.</span><span class="sxs-lookup"><span data-stu-id="47d9b-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="47d9b-136">[![Wywoływanie metody NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="47d9b-136">[![Invoking a NonAction method](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span></span>

<span data-ttu-id="47d9b-137">**Rysunek 01**: wywoływanie metody NonAction ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-an-action-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="47d9b-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-cs/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="47d9b-138">[Poprzednie](creating-a-controller-cs.md)
> [dalej](asp-net-mvc-routing-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="47d9b-138">[Previous](creating-a-controller-cs.md)
[Next](asp-net-mvc-routing-overview-vb.md)</span></span>
