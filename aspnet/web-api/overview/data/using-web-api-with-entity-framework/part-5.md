---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Tworzenie obiektów transferu danych (dto) | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: a1be1b72559aaffdf530402313f58d6c2e25b189
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828166"
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="c677b-102">Tworzenie obiektów transferu danych (dto)</span><span class="sxs-lookup"><span data-stu-id="c677b-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="c677b-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c677b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c677b-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="c677b-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="c677b-105">Po prawej stronie teraz nasz internetowy interfejs API jednostek bazy danych do klienta.</span><span class="sxs-lookup"><span data-stu-id="c677b-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="c677b-106">Klient odbiera dane, które mapuje bezpośrednio na tabelach bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c677b-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="c677b-107">Jednakże, które nie zawsze jest dobrym pomysłem.</span><span class="sxs-lookup"><span data-stu-id="c677b-107">However, that's not always a good idea.</span></span> <span data-ttu-id="c677b-108">Czasami trzeba zmienić kształt danych, który można wysłać do klienta.</span><span class="sxs-lookup"><span data-stu-id="c677b-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="c677b-109">Na przykład możesz chcieć:</span><span class="sxs-lookup"><span data-stu-id="c677b-109">For example, you might want to:</span></span>

- <span data-ttu-id="c677b-110">Usuń odwołania cykliczne (zobacz poprzednią sekcję).</span><span class="sxs-lookup"><span data-stu-id="c677b-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="c677b-111">Ukryj konkretnej właściwości, które klienci nie powinni wyświetlić.</span><span class="sxs-lookup"><span data-stu-id="c677b-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="c677b-112">Pominięto niektóre właściwości, aby zmniejszyć rozmiar ładunku.</span><span class="sxs-lookup"><span data-stu-id="c677b-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="c677b-113">Spłaszczanie wykresów obiektów, które zawierają obiektów zagnieżdżonych, aby były bardziej wygodne do klientów.</span><span class="sxs-lookup"><span data-stu-id="c677b-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="c677b-114">Należy unikać "polegającymi" luki w zabezpieczeniach.</span><span class="sxs-lookup"><span data-stu-id="c677b-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="c677b-115">(Zobacz [sprawdzania poprawności modelu](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) dyskusję na temat nadmiernego ogłaszania.)</span><span class="sxs-lookup"><span data-stu-id="c677b-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="c677b-116">Rozdzielenie swojej warstwy usług z warstwą bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c677b-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="c677b-117">Aby to osiągnąć, można zdefiniować *obiekt transferu danych* (DTO).</span><span class="sxs-lookup"><span data-stu-id="c677b-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="c677b-118">Obiekt DTO jest obiektem, który definiuje, jak dane zostaną wysłane za pośrednictwem sieci.</span><span class="sxs-lookup"><span data-stu-id="c677b-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="c677b-119">Zobaczmy, jak to działa z jednostką książki.</span><span class="sxs-lookup"><span data-stu-id="c677b-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="c677b-120">W folderze modeli należy dodać dwie klasy DTO:</span><span class="sxs-lookup"><span data-stu-id="c677b-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="c677b-121">`BookDetailDTO` Klasa zawiera wszystkie właściwości modelu książki, chyba że `AuthorName` jest ciągiem, który będzie przechowywać imię i nazwisko autora.</span><span class="sxs-lookup"><span data-stu-id="c677b-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="c677b-122">`BookDTO` Klasy zawiera podzbiór właściwości z `BookDetailDTO`.</span><span class="sxs-lookup"><span data-stu-id="c677b-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="c677b-123">Następnie zastąp te dwie metody GET w `BooksController` klasy z wersjami, które zwracają dto.</span><span class="sxs-lookup"><span data-stu-id="c677b-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="c677b-124">Użyjemy LINQ **wybierz** instrukcję, aby konwertować z książki jednostek dto.</span><span class="sxs-lookup"><span data-stu-id="c677b-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="c677b-125">Oto SQL generowane przez nowy `GetBooks` metody.</span><span class="sxs-lookup"><span data-stu-id="c677b-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="c677b-126">Widać, że EF tłumaczy LINQ **wybierz** w instrukcji SQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="c677b-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="c677b-127">Na koniec zmodyfikuj `PostBook` metodę, aby zwrócić obiekt DTO.</span><span class="sxs-lookup"><span data-stu-id="c677b-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="c677b-128">W tym samouczku konwertujemy na dto ręcznie w kodzie.</span><span class="sxs-lookup"><span data-stu-id="c677b-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="c677b-129">Innym rozwiązaniem jest korzystanie z biblioteki, takich jak [AutoMapper](http://automapper.org/) automatycznie obsługująca konwersji.</span><span class="sxs-lookup"><span data-stu-id="c677b-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="c677b-130">[Poprzednie](part-4.md)
> [dalej](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="c677b-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
