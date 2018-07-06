---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Tworzenie widoku (UI) | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: eeaa36ede45b82afd112a270acf68105d1ae6dbc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820989"
---
<a name="create-the-view-ui"></a><span data-ttu-id="d06ce-102">Tworzenie widoku (UI)</span><span class="sxs-lookup"><span data-stu-id="d06ce-102">Create the View (UI)</span></span>
====================
<span data-ttu-id="d06ce-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d06ce-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d06ce-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="d06ce-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="d06ce-105">W tej sekcji powoduje rozpoczęcie zdefiniować kod HTML dla aplikacji, a następnie dodać powiązanie danych pomiędzy HTML a model widoku.</span><span class="sxs-lookup"><span data-stu-id="d06ce-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="d06ce-106">Otwórz plik Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="d06ce-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="d06ce-107">Zamień całą zawartość tego pliku poniżej.</span><span class="sxs-lookup"><span data-stu-id="d06ce-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="d06ce-108">Większość `div` elementy są [Bootstrap](http://getbootstrap.com/) style.</span><span class="sxs-lookup"><span data-stu-id="d06ce-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="d06ce-109">Ważne elementy są tymi, które z `data-bind` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="d06ce-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="d06ce-110">Ten atrybut łączy HTML w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="d06ce-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="d06ce-111">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d06ce-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="d06ce-112">W tym przykładzie &quot; `text` &quot; powoduje, że powiązanie `<p>` elementu, aby wyświetlić wartość `error` właściwości z modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="d06ce-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="d06ce-113">Pamiętamy `error` został zadeklarowany jako `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="d06ce-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="d06ce-114">Zawsze, gdy nowa wartość jest przypisany do `error`, Knockout aktualizuje tekstu w `<p>` elementu.</span><span class="sxs-lookup"><span data-stu-id="d06ce-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="d06ce-115">`foreach` Powiązania informuje Knockout pętli zawartość `books` tablicy.</span><span class="sxs-lookup"><span data-stu-id="d06ce-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="d06ce-116">Dla każdego elementu w tablicy, tworzy nową Knockout &lt;li&gt; elementu.</span><span class="sxs-lookup"><span data-stu-id="d06ce-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="d06ce-117">Powiązania w kontekście `foreach` odnoszą się do właściwości dla elementu tablicy.</span><span class="sxs-lookup"><span data-stu-id="d06ce-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="d06ce-118">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="d06ce-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="d06ce-119">W tym miejscu `text` powiązania odczytuje właściwość Autor poszczególne książki.</span><span class="sxs-lookup"><span data-stu-id="d06ce-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="d06ce-120">Jeśli uruchomisz aplikację teraz, jego powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="d06ce-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="d06ce-121">Listy książek ładuje asynchronicznie, po załadowaniu strony.</span><span class="sxs-lookup"><span data-stu-id="d06ce-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="d06ce-122">Od razu &quot;szczegóły&quot; nie działają łącza.</span><span class="sxs-lookup"><span data-stu-id="d06ce-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="d06ce-123">Ta funkcja zostanie dodany w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="d06ce-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d06ce-124">[Poprzednie](part-6.md)
> [dalej](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="d06ce-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
