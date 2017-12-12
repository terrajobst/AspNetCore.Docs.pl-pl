---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: "Otwórz typy w protokołu OData v4 z interfejsu API sieci Web programu ASP.NET | Dokumentacja firmy Microsoft"
author: microsoft
description: "W protokołu OData v4 otwartym typem jest typ stuctured, który zawiera właściwości dynamicznych, oprócz żadnych właściwości, które są zadeklarowane w definicji typu. Otwórz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: c2d7454534ff0e9e0a80365793800ab7c45d3b6e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="14daa-104">Otwórz typy w protokołu OData v4 z interfejsu API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="14daa-104">Open Types in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="14daa-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="14daa-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="14daa-106">W przypadku protokołu OData v4 *otwartym typem* jest typu stuctured, który zawiera właściwości dynamicznych, oprócz żadnych właściwości, które są zadeklarowane w definicji typu.</span><span class="sxs-lookup"><span data-stu-id="14daa-106">In OData v4, an *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="14daa-107">Otwórz typy umożliwiają bardziej elastyczne modeli danych.</span><span class="sxs-lookup"><span data-stu-id="14daa-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="14daa-108">Ten samouczek przedstawia sposób użycia typy otwarte w programie ASP.NET Web API OData.</span><span class="sxs-lookup"><span data-stu-id="14daa-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="14daa-109">Ten samouczek zakłada, że już wiesz, jak można utworzyć punktu końcowego OData w interfejsie API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="14daa-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="14daa-110">Jeśli nie, Rozpocznij od przeczytania [utworzyć punktu końcowego OData v4](create-an-odata-v4-endpoint.md) pierwszy.</span><span class="sxs-lookup"><span data-stu-id="14daa-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="14daa-111">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="14daa-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="14daa-112">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="14daa-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="14daa-113">Protokołu OData v4</span><span class="sxs-lookup"><span data-stu-id="14daa-113">OData v4</span></span>


<span data-ttu-id="14daa-114">Pierwszy, niektóre terminologii OData:</span><span class="sxs-lookup"><span data-stu-id="14daa-114">First, some OData terminology:</span></span>

- <span data-ttu-id="14daa-115">Typ jednostki: typem strukturalnym przy użyciu klucza.</span><span class="sxs-lookup"><span data-stu-id="14daa-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="14daa-116">Typ złożony: typem strukturalnym bez klucza.</span><span class="sxs-lookup"><span data-stu-id="14daa-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="14daa-117">Otwórz typu: typ z właściwości dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="14daa-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="14daa-118">Zarówno typów jednostek i typów złożonych może być otwarty.</span><span class="sxs-lookup"><span data-stu-id="14daa-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="14daa-119">Wartość właściwości dynamicznych może być typu pierwotnego, typu złożonego lub typem wyliczenia; lub kolekcji żadnego z tych typów.</span><span class="sxs-lookup"><span data-stu-id="14daa-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="14daa-120">Aby uzyskać więcej informacji na temat typów open zobacz [specyfikację OData v4](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="14daa-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="14daa-121">Zainstaluj biblioteki OData sieci Web</span><span class="sxs-lookup"><span data-stu-id="14daa-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="14daa-122">Użyj Menedżera pakietów NuGet do zainstalowania najnowszej bibliotek Web API OData.</span><span class="sxs-lookup"><span data-stu-id="14daa-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="14daa-123">W oknie Konsola Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="14daa-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="14daa-124">Definiowanie typów CLR</span><span class="sxs-lookup"><span data-stu-id="14daa-124">Define the CLR Types</span></span>

<span data-ttu-id="14daa-125">Rozpocznij od zdefiniowania modelach EDM jako typów CLR.</span><span class="sxs-lookup"><span data-stu-id="14daa-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="14daa-126">Podczas tworzenia modelu danych jednostki (EDM)</span><span class="sxs-lookup"><span data-stu-id="14daa-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="14daa-127">`Category`jest typem wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="14daa-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="14daa-128">`Address`jest to typ złożony.</span><span class="sxs-lookup"><span data-stu-id="14daa-128">`Address` is a complex type.</span></span> <span data-ttu-id="14daa-129">(Nie ma klucza, więc nie jest typem jednostki.)</span><span class="sxs-lookup"><span data-stu-id="14daa-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="14daa-130">`Customer`nie jest typem jednostki.</span><span class="sxs-lookup"><span data-stu-id="14daa-130">`Customer` is an entity type.</span></span> <span data-ttu-id="14daa-131">(Ma klucz).</span><span class="sxs-lookup"><span data-stu-id="14daa-131">(It has a key.)</span></span>
- <span data-ttu-id="14daa-132">`Press`jest otwarty typ złożony.</span><span class="sxs-lookup"><span data-stu-id="14daa-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="14daa-133">`Book`nie jest typem jednostki otwarte.</span><span class="sxs-lookup"><span data-stu-id="14daa-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="14daa-134">Aby utworzyć typu otwartego, typ CLR musi mieć właściwość typu `IDictionary<string, object>`, które zawiera właściwości dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="14daa-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="14daa-135">Tworzenie modelu EDM</span><span class="sxs-lookup"><span data-stu-id="14daa-135">Build the EDM Model</span></span>

<span data-ttu-id="14daa-136">Jeśli używasz **ODataConventionModelBuilder** utworzyć EDM, `Press` i `Book` są automatycznie dodawane jako typów open, na podstawie obecności z `IDictionary<string, object>` właściwości.</span><span class="sxs-lookup"><span data-stu-id="14daa-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="14daa-137">Można również tworzyć EDM jawnie, przy użyciu **element ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="14daa-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="14daa-138">Dodawanie kontrolera OData</span><span class="sxs-lookup"><span data-stu-id="14daa-138">Add an OData Controller</span></span>

<span data-ttu-id="14daa-139">Następnie dodaj kontrolerze OData.</span><span class="sxs-lookup"><span data-stu-id="14daa-139">Next, add an OData controller.</span></span> <span data-ttu-id="14daa-140">W tym samouczku użyjemy uproszczony kontroler obsługuje tylko GET i POST żądań czy używa listy w pamięci do przechowywania obiektów.</span><span class="sxs-lookup"><span data-stu-id="14daa-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="14daa-141">Należy zauważyć, że pierwszy `Book` wystąpienie nie ma dynamicznych właściwości.</span><span class="sxs-lookup"><span data-stu-id="14daa-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="14daa-142">Drugi `Book` wystąpienie ma następujące właściwości dynamicznych:</span><span class="sxs-lookup"><span data-stu-id="14daa-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="14daa-143">"Opublikowane": typ pierwotny</span><span class="sxs-lookup"><span data-stu-id="14daa-143">"Published": Primitive type</span></span>
- <span data-ttu-id="14daa-144">"Autorzy": zbiór typy pierwotne</span><span class="sxs-lookup"><span data-stu-id="14daa-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="14daa-145">"OtherCategories": kolekcję typów wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="14daa-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="14daa-146">Ponadto `Press` właściwości tego `Book` wystąpienie ma następujące właściwości dynamicznych:</span><span class="sxs-lookup"><span data-stu-id="14daa-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="14daa-147">"Blog": typ pierwotny</span><span class="sxs-lookup"><span data-stu-id="14daa-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="14daa-148">"Address": typ złożony</span><span class="sxs-lookup"><span data-stu-id="14daa-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="14daa-149">Zapytanie metadanych</span><span class="sxs-lookup"><span data-stu-id="14daa-149">Query the Metadata</span></span>

<span data-ttu-id="14daa-150">Aby uzyskać ten dokument metadanych OData, Wyślij żądanie GET `~/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="14daa-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="14daa-151">Treść odpowiedzi powinny wyglądać podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="14daa-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="14daa-152">Z dokumentu metadanych można stwierdzić, że:</span><span class="sxs-lookup"><span data-stu-id="14daa-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="14daa-153">Aby uzyskać `Book` i `Press` typów wartości `OpenType` atrybut ma wartość true.</span><span class="sxs-lookup"><span data-stu-id="14daa-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="14daa-154">`Customer` i `Address` typów nie ma tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="14daa-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="14daa-155">`Book` Typ jednostki ma trzy zadeklarowane właściwości: ISBN, tytuł i naciśnij klawisz.</span><span class="sxs-lookup"><span data-stu-id="14daa-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="14daa-156">Nie ma metadanych OData `Book.Properties` właściwość z klasy CLR.</span><span class="sxs-lookup"><span data-stu-id="14daa-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="14daa-157">Podobnie `Press` typu złożonego ma tylko dwie właściwości zadeklarowane: nazwą i kategorią.</span><span class="sxs-lookup"><span data-stu-id="14daa-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="14daa-158">Metadane nie obejmuje `Press.DynamicProperties` właściwość z klasy CLR.</span><span class="sxs-lookup"><span data-stu-id="14daa-158">The metadata does not not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="14daa-159">Zapytanie jednostki</span><span class="sxs-lookup"><span data-stu-id="14daa-159">Query an Entity</span></span>

<span data-ttu-id="14daa-160">Aby uzyskać książkę o ISBN równa "978-0-7356-7942-9", wysyłania Wyślij żądanie GET `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="14daa-160">To get the book with ISBN equal to "978-0-7356-7942-9", send send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="14daa-161">Treść odpowiedzi powinien wyglądać podobnie do następującego.</span><span class="sxs-lookup"><span data-stu-id="14daa-161">The response body should look similar to the following.</span></span> <span data-ttu-id="14daa-162">(Wcięty, aby był bardziej czytelny.)</span><span class="sxs-lookup"><span data-stu-id="14daa-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="14daa-163">Zwróć uwagę, że właściwości dynamicznych są uwzględniane wbudowany z zadeklarowane właściwości.</span><span class="sxs-lookup"><span data-stu-id="14daa-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="14daa-164">POST jednostki</span><span class="sxs-lookup"><span data-stu-id="14daa-164">POST an Entity</span></span>

<span data-ttu-id="14daa-165">Aby dodać jednostkę książki, Wyślij żądanie POST `~/Books`.</span><span class="sxs-lookup"><span data-stu-id="14daa-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="14daa-166">Klienta można ustawić właściwości dynamicznych w ładunku żądania.</span><span class="sxs-lookup"><span data-stu-id="14daa-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="14daa-167">Oto przykładowe żądanie.</span><span class="sxs-lookup"><span data-stu-id="14daa-167">Here is an example request.</span></span> <span data-ttu-id="14daa-168">Zanotuj właściwości "Price" i "Opublikowaną".</span><span class="sxs-lookup"><span data-stu-id="14daa-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="14daa-169">Jeśli ustawisz punkt przerwania w metodzie kontrolera widać dodania tych właściwości do interfejsu API sieci Web `Properties` słownika.</span><span class="sxs-lookup"><span data-stu-id="14daa-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="14daa-170">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="14daa-170">Additional Resources</span></span>

[<span data-ttu-id="14daa-171">Przykładowe typu OData Open</span><span class="sxs-lookup"><span data-stu-id="14daa-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
