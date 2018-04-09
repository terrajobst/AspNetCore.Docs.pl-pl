---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Część 2: Tworzenie modeli domeny | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 84631494c1be266c21e5e5702182df717b1d29b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-2-creating-the-domain-models"></a><span data-ttu-id="30cf9-102">Część 2: Tworzenie modeli domeny</span><span class="sxs-lookup"><span data-stu-id="30cf9-102">Part 2: Creating the Domain Models</span></span>
====================
<span data-ttu-id="30cf9-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="30cf9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="30cf9-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="30cf9-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="30cf9-105">Dodawanie modeli</span><span class="sxs-lookup"><span data-stu-id="30cf9-105">Add Models</span></span>

<span data-ttu-id="30cf9-106">Istnieją trzy sposoby podejście Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="30cf9-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="30cf9-107">Pierwszy bazy danych: rozpoczyna się z bazą danych oraz Entity Framework generuje kod.</span><span class="sxs-lookup"><span data-stu-id="30cf9-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="30cf9-108">Pierwszy modelu: zaczynać visual modelu i generuje Entity Framework, bazy danych i kodu.</span><span class="sxs-lookup"><span data-stu-id="30cf9-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="30cf9-109">Pierwszy kod: Uruchom z kodem i Entity Framework generuje bazy danych.</span><span class="sxs-lookup"><span data-stu-id="30cf9-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="30cf9-110">Użyto podejście pierwszy kod, więc Rozpoczniemy definiując naszych obiektów domeny jako POCOs (stary zwykły obiektów CLR).</span><span class="sxs-lookup"><span data-stu-id="30cf9-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="30cf9-111">Podejście pierwszy kod obiektów domeny nie wymagają żadnych dodatkowych kodu do obsługi warstwy bazy danych, takich jak trwałości lub transakcji.</span><span class="sxs-lookup"><span data-stu-id="30cf9-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="30cf9-112">(W szczególności nie muszą dziedziczyć [typu EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) klasy.) Można nadal używać adnotacji danych do kontrolowania sposobu Entity Framework utworzy schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="30cf9-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="30cf9-113">Ponieważ POCOs nie zawierają żadnych dodatkowych właściwości opisujących [bazy danych stanu](https://msdn.microsoft.com/library/system.data.entitystate.aspx), łatwo może być Zserializowany do formatu JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="30cf9-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="30cf9-114">Jednak, który oznacza to, że zawsze powinny ujawniać modeli Entity Framework bezpośrednio do klientów, jak zajmiemy się później w samouczku.</span><span class="sxs-lookup"><span data-stu-id="30cf9-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="30cf9-115">Utworzymy POCOs następujące:</span><span class="sxs-lookup"><span data-stu-id="30cf9-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="30cf9-116">Produkt</span><span class="sxs-lookup"><span data-stu-id="30cf9-116">Product</span></span>
- <span data-ttu-id="30cf9-117">Kolejność</span><span class="sxs-lookup"><span data-stu-id="30cf9-117">Order</span></span>
- <span data-ttu-id="30cf9-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="30cf9-118">OrderDetail</span></span>

<span data-ttu-id="30cf9-119">Do tworzenia każdej klasy, kliknij prawym przyciskiem myszy folder modeli w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="30cf9-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="30cf9-120">Wybierz z menu kontekstowego **Dodaj** , a następnie wybierz **klasy.**</span><span class="sxs-lookup"><span data-stu-id="30cf9-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="30cf9-121">Dodaj `Product` klas z implementacji następujących:</span><span class="sxs-lookup"><span data-stu-id="30cf9-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="30cf9-122">Według Konwencji, korzysta z programu Entity Framework `Id` właściwość jako klucz podstawowy i mapuje go do kolumny tożsamości w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="30cf9-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="30cf9-123">Podczas tworzenia nowego `Product` wystąpienia, nie będzie ustaw wartość `Id`, ponieważ wartość generuje bazy danych.</span><span class="sxs-lookup"><span data-stu-id="30cf9-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="30cf9-124">**ScaffoldColumn** atrybutu poinformuje platformę ASP.NET MVC, aby pominąć `Id` właściwości podczas generowania formularza edytora.</span><span class="sxs-lookup"><span data-stu-id="30cf9-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="30cf9-125">**Wymagane** atrybut służy do sprawdzania poprawności modelu.</span><span class="sxs-lookup"><span data-stu-id="30cf9-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="30cf9-126">Określa, że `Name` właściwość musi być niepustym ciągiem.</span><span class="sxs-lookup"><span data-stu-id="30cf9-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="30cf9-127">Dodaj `Order` klasy:</span><span class="sxs-lookup"><span data-stu-id="30cf9-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="30cf9-128">Dodaj `OrderDetail` klasy:</span><span class="sxs-lookup"><span data-stu-id="30cf9-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="30cf9-129">Relacje klucza obcego</span><span class="sxs-lookup"><span data-stu-id="30cf9-129">Foreign Key Relations</span></span>

<span data-ttu-id="30cf9-130">Kolejność zawiera wiele szczegółów zamówienia, a każdy szczegółami zamówienia odwołuje się do pojedynczego produktu.</span><span class="sxs-lookup"><span data-stu-id="30cf9-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="30cf9-131">Do reprezentowania tych relacji `OrderDetail` klasy definiuje właściwości o nazwie `OrderId` i `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="30cf9-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="30cf9-132">Entity Framework wywnioskuje te właściwości reprezentują kluczy obcych i doda ograniczenia klucza obcego do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="30cf9-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="30cf9-133">`Order` i `OrderDetail` klasy również zawierać właściwości "nawigacji", które zawierają odwołania do powiązanych obiektów.</span><span class="sxs-lookup"><span data-stu-id="30cf9-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="30cf9-134">Podana kolejności, można przejść do produktów w kolejności, wykonując właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="30cf9-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="30cf9-135">Teraz skompilować projekt.</span><span class="sxs-lookup"><span data-stu-id="30cf9-135">Compile the project now.</span></span> <span data-ttu-id="30cf9-136">Entity Framework używa odbicia do odnajdywania właściwości modeli, dzięki czemu wymaga skompilowanym zestawie utworzyć schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="30cf9-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="30cf9-137">Konfiguruj programy formatujące typy nośnika</span><span class="sxs-lookup"><span data-stu-id="30cf9-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="30cf9-138">A [program formatujący typ nośnika](../../formats-and-model-binding/media-formatters.md) to obiekt, który serializuje dane, gdy interfejs API sieci Web zapisuje treści odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="30cf9-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="30cf9-139">Wbudowane elementy formatujące obsługuje dane wyjściowe JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="30cf9-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="30cf9-140">Domyślnie wszystkie obiekty oba te elementy formatujące serializować przez wartość.</span><span class="sxs-lookup"><span data-stu-id="30cf9-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="30cf9-141">Serializacja przez wartość tworzy problem, jeśli obiekt zawiera cykliczne odwołanie.</span><span class="sxs-lookup"><span data-stu-id="30cf9-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="30cf9-142">To dokładnie w przypadku `Order` i `OrderDetail` klas, ponieważ każdy zawiera odwołanie do drugiego.</span><span class="sxs-lookup"><span data-stu-id="30cf9-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="30cf9-143">Program formatujący wykonaj odwołań, zapisywanie każdego obiektu według wartości i go w programie okręgi.</span><span class="sxs-lookup"><span data-stu-id="30cf9-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="30cf9-144">W związku z tym należy zmienić to zachowanie domyślne.</span><span class="sxs-lookup"><span data-stu-id="30cf9-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="30cf9-145">W Eksploratorze rozwiązań rozwiń aplikacji\_Uruchom folderu i Otwórz plik o nazwie WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="30cf9-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="30cf9-146">Dodaj następujący kod do `WebApiConfig` klasy:</span><span class="sxs-lookup"><span data-stu-id="30cf9-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="30cf9-147">Ten kod ustawia element formatujący JSON, aby zachować odwołania do obiektów i całkowicie usuwa element formatujący XML z potoku.</span><span class="sxs-lookup"><span data-stu-id="30cf9-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="30cf9-148">(Można skonfigurować element formatujący XML, aby zachować odwołania do obiektów, ale jest nieco więcej pracy i musimy tylko JSON dla tej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="30cf9-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="30cf9-149">Aby uzyskać więcej informacji, zobacz [obsługi odwołań cyklicznych obiektu](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="30cf9-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="30cf9-150">[Poprzednie](using-web-api-with-entity-framework-part-1.md)
> [dalej](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="30cf9-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
