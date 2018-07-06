---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Część 2: Tworzenie modeli domeny | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 605a3eb5d781db6b317369efe8698288d7b9f648
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802273"
---
<a name="part-2-creating-the-domain-models"></a><span data-ttu-id="9f291-102">Część 2: Tworzenie modeli domeny</span><span class="sxs-lookup"><span data-stu-id="9f291-102">Part 2: Creating the Domain Models</span></span>
====================
<span data-ttu-id="9f291-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9f291-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="9f291-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="9f291-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="9f291-105">Dodawanie modeli</span><span class="sxs-lookup"><span data-stu-id="9f291-105">Add Models</span></span>

<span data-ttu-id="9f291-106">Istnieją trzy sposoby podejścia platformy Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="9f291-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="9f291-107">Bazy danych: Uruchom z bazą danych i Entity Framework generuje kod.</span><span class="sxs-lookup"><span data-stu-id="9f291-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="9f291-108">Pierwszy model: Uruchom z modelem visual i Entity Framework generuje bazy danych i kod.</span><span class="sxs-lookup"><span data-stu-id="9f291-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="9f291-109">Najpierw kod: Uruchom z kodem i Entity Framework generuje bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9f291-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="9f291-110">Używamy podejście najpierw kod, dzięki czemu możemy rozpocząć od zdefiniowania naszych obiektów domeny jako POCOs (stary zwykły obiektów CLR).</span><span class="sxs-lookup"><span data-stu-id="9f291-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="9f291-111">W przypadku najpierw kod metody obiektów domeny nie ma potrzeby wprowadzania dodatkowego kodu do obsługi warstwy bazy danych, takich jak transakcje lub trwałość.</span><span class="sxs-lookup"><span data-stu-id="9f291-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="9f291-112">(W szczególności nie musi dziedziczyć z [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) klasy.) Korzystanie z adnotacji danych, można nadal kontrolować, jak Entity Framework tworzy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9f291-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="9f291-113">Ponieważ POCOs nie mają żadnych dodatkowych właściwości, które opisują [bazy danych stanu](https://msdn.microsoft.com/library/system.data.entitystate.aspx), łatwo może być serializowany do formatu JSON lub XML.</span><span class="sxs-lookup"><span data-stu-id="9f291-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="9f291-114">Jednakże, nie znaczy, że zawsze należy udostępnić swoje modele platformy Entity Framework bezpośrednio do klientów, jak opisano w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="9f291-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="9f291-115">Utworzymy POCOs następujące:</span><span class="sxs-lookup"><span data-stu-id="9f291-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="9f291-116">Produkt</span><span class="sxs-lookup"><span data-stu-id="9f291-116">Product</span></span>
- <span data-ttu-id="9f291-117">Kolejność</span><span class="sxs-lookup"><span data-stu-id="9f291-117">Order</span></span>
- <span data-ttu-id="9f291-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="9f291-118">OrderDetail</span></span>

<span data-ttu-id="9f291-119">Aby utworzyć każdej klasy, kliknij prawym przyciskiem myszy folder modeli w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="9f291-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="9f291-120">Z menu kontekstowego wybierz **Dodaj** , a następnie wybierz **klasy.**</span><span class="sxs-lookup"><span data-stu-id="9f291-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="9f291-121">Dodaj `Product` klasy następującą implementacją:</span><span class="sxs-lookup"><span data-stu-id="9f291-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="9f291-122">Zgodnie z Konwencją, używa platformy Entity Framework `Id` właściwość jako klucz podstawowy i jego mapuje kolumną tożsamości w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9f291-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="9f291-123">Podczas tworzenia nowego `Product` wystąpienia nie ustawisz wartość `Id`, ponieważ baza danych generuje wartość.</span><span class="sxs-lookup"><span data-stu-id="9f291-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="9f291-124">**ScaffoldColumn** atrybut poinformuje platformę ASP.NET MVC, aby pominąć `Id` właściwości podczas generowania formie edytora.</span><span class="sxs-lookup"><span data-stu-id="9f291-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="9f291-125">**Wymagane** atrybut jest używany do sprawdzania poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="9f291-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="9f291-126">Określa, że `Name` właściwość musi być niepustym ciągiem.</span><span class="sxs-lookup"><span data-stu-id="9f291-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="9f291-127">Dodaj `Order` klasy:</span><span class="sxs-lookup"><span data-stu-id="9f291-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="9f291-128">Dodaj `OrderDetail` klasy:</span><span class="sxs-lookup"><span data-stu-id="9f291-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="9f291-129">Relacje klucza obcego</span><span class="sxs-lookup"><span data-stu-id="9f291-129">Foreign Key Relations</span></span>

<span data-ttu-id="9f291-130">Zamówienie zawiera wiele szczegółów zamówienia, a każdy szczegół kolejności odwołuje się do jednego produktu.</span><span class="sxs-lookup"><span data-stu-id="9f291-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="9f291-131">Do reprezentowania te relacje `OrderDetail` klasy definiuje właściwości o nazwie `OrderId` i `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="9f291-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="9f291-132">Entity Framework wywnioskuje te właściwości reprezentują kluczy obcych i doda ograniczenia foreign key z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="9f291-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="9f291-133">`Order` i `OrderDetail` klasy również zawierać właściwości "navigation", które zawierają odwołania do powiązanych obiektów.</span><span class="sxs-lookup"><span data-stu-id="9f291-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="9f291-134">Biorąc pod uwagę zamówienie, można przejść do produktów w kolejności, postępując zgodnie z właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="9f291-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="9f291-135">Skompiluj teraz projekt.</span><span class="sxs-lookup"><span data-stu-id="9f291-135">Compile the project now.</span></span> <span data-ttu-id="9f291-136">Entity Framework używa odbicia, aby odnaleźć właściwości te modele, dzięki czemu wymaga skompilowanego zestawu w celu utworzenia schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9f291-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="9f291-137">Konfiguruj programy formatujące typy nośnika</span><span class="sxs-lookup"><span data-stu-id="9f291-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="9f291-138">A [program formatujący typ nośnika](../../formats-and-model-binding/media-formatters.md) jest obiektem, który serializuje dane, gdy interfejs API sieci Web zapisuje treści odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="9f291-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="9f291-139">Wbudowane elementy formatujące obsługuje dane wyjściowe JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="9f291-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="9f291-140">Domyślnie oba te elementy formatujące serializować wszystkich obiektów według wartości.</span><span class="sxs-lookup"><span data-stu-id="9f291-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="9f291-141">Serializacja według wartości tworzy problem, jeśli wykresu obiektu zawiera odwołania cykliczne.</span><span class="sxs-lookup"><span data-stu-id="9f291-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="9f291-142">Dokładnie tak, za pomocą `Order` i `OrderDetail` klasy, ponieważ każdy zawiera odwołanie do drugiego.</span><span class="sxs-lookup"><span data-stu-id="9f291-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="9f291-143">Element formatujący wykonaj odwołań, zapisywanie każdy obiekt przez wartość i go w programie koła.</span><span class="sxs-lookup"><span data-stu-id="9f291-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="9f291-144">W związku z tym należy zmienić zachowanie domyślne.</span><span class="sxs-lookup"><span data-stu-id="9f291-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="9f291-145">W Eksploratorze rozwiązań rozwiń aplikacji\_Uruchom folder i Otwórz plik o nazwie WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="9f291-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="9f291-146">Dodaj następujący kod do `WebApiConfig` klasy:</span><span class="sxs-lookup"><span data-stu-id="9f291-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="9f291-147">Ten kod ustawia element formatujący JSON, aby zachować odwołania do obiektu i całkowicie usunie element formatujący XML z potoku.</span><span class="sxs-lookup"><span data-stu-id="9f291-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="9f291-148">(Można skonfigurować program formatujący kod XML, aby zachować odwołania do obiektu, ale jest nieco więcej pracy, a potrzebujemy tylko JSON dla tej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9f291-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="9f291-149">Aby uzyskać więcej informacji, zobacz [obsługi odwołań cyklicznych obiektu](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="9f291-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9f291-150">[Poprzednie](using-web-api-with-entity-framework-part-1.md)
> [dalej](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="9f291-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
