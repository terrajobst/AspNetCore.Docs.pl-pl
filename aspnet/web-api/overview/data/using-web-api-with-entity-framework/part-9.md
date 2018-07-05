---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Dodaj nowy element do bazy danych | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: b1f7935c70efcc3ee486e76fc356ff43716632dd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368880"
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="2e068-102">Dodaj nowy element do bazy danych</span><span class="sxs-lookup"><span data-stu-id="2e068-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="2e068-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2e068-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2e068-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="2e068-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="2e068-105">W tej sekcji dodasz możliwości dla użytkowników w celu utworzenia nowej książki.</span><span class="sxs-lookup"><span data-stu-id="2e068-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="2e068-106">W app.js Dodaj następujący kod do modelu widoku:</span><span class="sxs-lookup"><span data-stu-id="2e068-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="2e068-107">W Index.cshtml Zastąp następujący kod:</span><span class="sxs-lookup"><span data-stu-id="2e068-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="2e068-108">Za pomocą:</span><span class="sxs-lookup"><span data-stu-id="2e068-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="2e068-109">Ten kod znaczników tworzy formularz przesyłania nowego autora.</span><span class="sxs-lookup"><span data-stu-id="2e068-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="2e068-110">Wartości na liście rozwijanej autor jest powiązany z danymi `authors` zauważalne w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="2e068-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="2e068-111">Dla innych nakładów formularza, wartości są powiązane z danymi do `newBook` właściwości modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="2e068-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="2e068-112">Obsługa przesyłania formularza jest powiązana z `addBook` funkcji:</span><span class="sxs-lookup"><span data-stu-id="2e068-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="2e068-113">`addBook` Funkcja odczytuje bieżące wartości składników powiązane z danymi formularza można utworzyć obiektu JSON.</span><span class="sxs-lookup"><span data-stu-id="2e068-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="2e068-114">A następnie wysyła on żądanie POST obiekt JSON służący do `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="2e068-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2e068-115">[Poprzednie](part-8.md)
> [dalej](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="2e068-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
