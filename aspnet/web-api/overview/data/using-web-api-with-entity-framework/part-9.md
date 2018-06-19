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
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 5845c092c4d7aee12b33b3f0a49c0e944c0fb9aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868376"
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="d0ed1-102">Dodaj nowy element do bazy danych</span><span class="sxs-lookup"><span data-stu-id="d0ed1-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="d0ed1-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d0ed1-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d0ed1-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="d0ed1-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="d0ed1-105">W tej sekcji zostaną dodane przez użytkowników w celu utworzenia nowej książki.</span><span class="sxs-lookup"><span data-stu-id="d0ed1-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="d0ed1-106">W app.js Dodaj następujący kod do modelu widoku:</span><span class="sxs-lookup"><span data-stu-id="d0ed1-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="d0ed1-107">W Index.cshtml Zastąp następujący kod:</span><span class="sxs-lookup"><span data-stu-id="d0ed1-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="d0ed1-108">Z:</span><span class="sxs-lookup"><span data-stu-id="d0ed1-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="d0ed1-109">Ten kod znaczników tworzy formularz przesyłania nowych autora.</span><span class="sxs-lookup"><span data-stu-id="d0ed1-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="d0ed1-110">Wartości na liście rozwijanej autora jest powiązany z danymi `authors` widocznych w modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="d0ed1-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="d0ed1-111">Do innych wejść formularza, wartości jest powiązany z danymi `newBook` właściwości modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="d0ed1-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="d0ed1-112">Obsługa przesyłania formularza jest powiązana z `addBook` funkcji:</span><span class="sxs-lookup"><span data-stu-id="d0ed1-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="d0ed1-113">`addBook` Funkcja odczytuje bieżące wartości dane wejściowe formularza powiązane z danymi można utworzyć obiektu JSON.</span><span class="sxs-lookup"><span data-stu-id="d0ed1-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="d0ed1-114">A następnie go zapisuje obiekt JSON do `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="d0ed1-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d0ed1-115">[Poprzednie](part-8.md)
> [dalej](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="d0ed1-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
