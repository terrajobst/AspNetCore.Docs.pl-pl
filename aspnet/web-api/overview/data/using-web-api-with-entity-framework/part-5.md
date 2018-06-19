---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Tworzenie obiektów Transfer danych (DTOs) | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 35e01f959072b96204de3e2ce3d507635720e110
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878688"
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="c8cb8-102">Tworzenie obiektów Transfer danych (DTOs)</span><span class="sxs-lookup"><span data-stu-id="c8cb8-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="c8cb8-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c8cb8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c8cb8-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="c8cb8-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="c8cb8-105">Prawo teraz naszych interfejsu API sieci web udostępnia jednostki bazy danych do klienta.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="c8cb8-106">Klient odbiera danych, która mapuje bezpośrednio do tabel bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="c8cb8-107">Jednak, że nie zawsze jest dobrym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-107">However, that's not always a good idea.</span></span> <span data-ttu-id="c8cb8-108">Czasami chcesz zmienić kształt danych, który możesz wysłać do klienta.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="c8cb8-109">Na przykład możesz chcieć:</span><span class="sxs-lookup"><span data-stu-id="c8cb8-109">For example, you might want to:</span></span>

- <span data-ttu-id="c8cb8-110">Usuń odwołania cykliczne (zobacz poprzedniej sekcji).</span><span class="sxs-lookup"><span data-stu-id="c8cb8-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="c8cb8-111">Ukryj konkretnej właściwości, które klienci nie powinni do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="c8cb8-112">Pominąć niektóre właściwości, aby zmniejszyć rozmiar ładunku.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="c8cb8-113">Spłaszczanie wykresów obiektów, zawierających zagnieżdżone obiekty, aby były wygodniejsze dla klientów.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="c8cb8-114">Unikaj "zbyt księgowej" luk w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="c8cb8-115">(Zobacz [sprawdzania poprawności modelu](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) omówienie publikowanie nadmierne.)</span><span class="sxs-lookup"><span data-stu-id="c8cb8-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="c8cb8-116">Oddziel z warstwy usług z warstwą bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="c8cb8-117">Aby to zrobić, można zdefiniować *obiektu transferu danych* (DTO).</span><span class="sxs-lookup"><span data-stu-id="c8cb8-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="c8cb8-118">DTO jest obiekt, który definiuje sposób dane będą wysyłane przez sieć.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="c8cb8-119">Zobaczmy, jak to zrobić z jednostką książki.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="c8cb8-120">W folderze modeli Dodaj dwie klasy DTO:</span><span class="sxs-lookup"><span data-stu-id="c8cb8-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="c8cb8-121">`BookDetailDTO` Klasa zawiera wszystkie właściwości modelu książki, z wyjątkiem `AuthorName` jest ciągiem, który będzie przechowywana nazwa autora.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="c8cb8-122">`BookDTO` Klasa zawiera podzbiór właściwości z `BookDetailDTO`.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="c8cb8-123">Następnie zastąp te dwie metody GET w `BooksController` klasy z wersjami, które zwracają DTOs.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="c8cb8-124">Użyjemy LINQ **wybierz** instrukcji, aby przekonwertować z jednostek książki DTOs.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="c8cb8-125">Oto SQL generowane przez nowy `GetBooks` metody.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="c8cb8-126">Widać, że EF tłumaczy LINQ **wybierz** w instrukcji SQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="c8cb8-127">Na koniec zmodyfikuj `PostBook` metodę, aby zwrócić DTO.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="c8cb8-128">W tym samouczku będziemy podczas konwertowania na DTOs ręcznie w kodzie.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="c8cb8-129">Innym rozwiązaniem jest korzystanie z biblioteki, takich jak [AutoMapper](http://automapper.org/) automatycznie obsługująca konwersji.</span><span class="sxs-lookup"><span data-stu-id="c8cb8-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="c8cb8-130">[Poprzednie](part-4.md)
> [dalej](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="c8cb8-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
