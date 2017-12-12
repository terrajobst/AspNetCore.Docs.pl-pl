---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: "Wskazówki dotyczące zabezpieczeń dla składnika ASP.NET Web API 2 OData | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 799e2a0c742b545acf3b5cd27531d734aa7def80
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="de9f9-102">Wskazówki dotyczące zabezpieczeń dla składnika ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="de9f9-102">Security Guidance for ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="de9f9-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="de9f9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="de9f9-104">W tym temacie opisano niektóre problemy z zabezpieczeniami, które należy rozważyć w przypadku ujawnienia zestawu danych za pośrednictwem OData.</span><span class="sxs-lookup"><span data-stu-id="de9f9-104">This topic describes some of the security issues that you should consider when exposing a dataset through OData.</span></span>

## <a name="edm-security"></a><span data-ttu-id="de9f9-105">EDM zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="de9f9-105">EDM Security</span></span>

<span data-ttu-id="de9f9-106">Semantyki zapytań są oparte na modelu danych jednostki (EDM), nie podstawowych typów modeli.</span><span class="sxs-lookup"><span data-stu-id="de9f9-106">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="de9f9-107">Właściwość można wykluczyć z EDM i nie będą widoczne w zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="de9f9-107">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="de9f9-108">Na przykład załóżmy, że model zawiera typ pracownika z właściwością wynagrodzenia.</span><span class="sxs-lookup"><span data-stu-id="de9f9-108">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="de9f9-109">Można wykluczyć tę właściwość z modelu EDM, aby je ukryć od klientów.</span><span class="sxs-lookup"><span data-stu-id="de9f9-109">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="de9f9-110">Istnieją dwa sposoby wyłączają właściwość z modelu EDM.</span><span class="sxs-lookup"><span data-stu-id="de9f9-110">There are two ways to exlude a property from the EDM.</span></span> <span data-ttu-id="de9f9-111">Można ustawić **[IgnoreDataMember]** atrybutu we właściwości w klasie modelu:</span><span class="sxs-lookup"><span data-stu-id="de9f9-111">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="de9f9-112">Można również usunąć właściwość z EDM programowo:</span><span class="sxs-lookup"><span data-stu-id="de9f9-112">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="de9f9-113">Zapytanie zabezpieczeń</span><span class="sxs-lookup"><span data-stu-id="de9f9-113">Query Security</span></span>

<span data-ttu-id="de9f9-114">Klient złośliwego lub prostego można utworzyć zapytanie, które bardzo długi czas do wykonania.</span><span class="sxs-lookup"><span data-stu-id="de9f9-114">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="de9f9-115">W najgorszym przypadku to przerwać dostęp do usługi.</span><span class="sxs-lookup"><span data-stu-id="de9f9-115">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="de9f9-116">**[Queryable]** atrybut jest filtr akcji, która analizuje, weryfikuje i stosuje zapytanie.</span><span class="sxs-lookup"><span data-stu-id="de9f9-116">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="de9f9-117">Filtr konwertuje opcje zapytania w wyrażeniu LINQ.</span><span class="sxs-lookup"><span data-stu-id="de9f9-117">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="de9f9-118">Gdy kontroler OData zwraca **IQueryable** typu **IQueryable** dostawcy LINQ Konwertuje wyrażenia LINQ na kwerendę.</span><span class="sxs-lookup"><span data-stu-id="de9f9-118">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="de9f9-119">W związku z tym wydajności zależy od dostawcy LINQ, który jest używany, a także na specyfiki schemat zestawu danych lub bazy danych.</span><span class="sxs-lookup"><span data-stu-id="de9f9-119">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="de9f9-120">Aby uzyskać więcej informacji o używaniu opcji zapytania OData w interfejsie API sieci Web ASP.NET, zobacz [obsługi opcji zapytania OData](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="de9f9-120">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="de9f9-121">Jeśli wiesz, że wszyscy klienci są zaufane (na przykład w środowisku przedsiębiorstwa) lub zestawu danych jest mały, wydajność zapytań nie może być problem.</span><span class="sxs-lookup"><span data-stu-id="de9f9-121">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="de9f9-122">W przeciwnym razie należy rozważyć poniższe zalecenia.</span><span class="sxs-lookup"><span data-stu-id="de9f9-122">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="de9f9-123">Testowanie usługi z różne zapytania i profilu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="de9f9-123">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="de9f9-124">Włącz stronicowanie oparte na serwerze uniknąć zwrócenie dużych zestawów danych w jednym zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="de9f9-124">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="de9f9-125">Aby uzyskać więcej informacji, zobacz [stronicowania Server-Driven](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="de9f9-125">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="de9f9-126">Potrzebujesz $filter i $orderby?</span><span class="sxs-lookup"><span data-stu-id="de9f9-126">Do you need $filter and $orderby?</span></span> <span data-ttu-id="de9f9-127">Niektóre aplikacje mogą umożliwić klienckim stronicowania, przy użyciu $top i $skip, ale wyłącz inne opcje zapytania.</span><span class="sxs-lookup"><span data-stu-id="de9f9-127">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="de9f9-128">Należy rozważyć ograniczenie $orderby do właściwości w indeks klastrowany.</span><span class="sxs-lookup"><span data-stu-id="de9f9-128">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="de9f9-129">Sortowanie dużej ilości danych bez indeksu klastrowanego jest powolne.</span><span class="sxs-lookup"><span data-stu-id="de9f9-129">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="de9f9-130">Liczba węzłów maksymalna: **właściwość MaxNodeCount** właściwość **[Queryable]** Ustawia maksymalną liczba węzłów dozwolone w drzewie składni $filter.</span><span class="sxs-lookup"><span data-stu-id="de9f9-130">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="de9f9-131">Wartość domyślna to 100, ale można ustawić niższą wartość, ponieważ dużej liczby węzłów może działać powoli skompilować.</span><span class="sxs-lookup"><span data-stu-id="de9f9-131">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="de9f9-132">Jest to szczególnie istotne w przypadku korzystania z LINQ do obiektów (np. zapytań LINQ w kolekcji w pamięci, bez korzystania z pośredniego dostawcy LINQ).</span><span class="sxs-lookup"><span data-stu-id="de9f9-132">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="de9f9-133">Rozważ wyłączenie funkcji any() i all(), jak mogą być powolne.</span><span class="sxs-lookup"><span data-stu-id="de9f9-133">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="de9f9-134">Jeśli wszystkie właściwości ciągu zawiera dużych ciągów & przykład #8212for, opis produktu lub wpis w blogu & #8212consider wyłączenie funkcji ciągów.</span><span class="sxs-lookup"><span data-stu-id="de9f9-134">If any string properties contain large strings&#8212for example, a product description or a blog entry&#8212consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="de9f9-135">Należy wziąć pod uwagę, brak zezwolenia filtrowanie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="de9f9-135">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="de9f9-136">Filtrowanie właściwości nawigacji może spowodować sprzężenia, który może być wolne w zależności od schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="de9f9-136">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="de9f9-137">Poniższy kod przedstawia moduł weryfikacji zapytania, który uniemożliwia filtrowanie właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="de9f9-137">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="de9f9-138">Aby uzyskać więcej informacji o moduły weryfikacji zapytań, zobacz [weryfikacji zapytań](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="de9f9-138">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="de9f9-139">Należy rozważyć ograniczenie zapytania $filter pisząc modułu sprawdzania poprawności, który jest dostosowany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="de9f9-139">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="de9f9-140">Na przykład wziąć pod uwagę następujące dwa zapytania:</span><span class="sxs-lookup"><span data-stu-id="de9f9-140">For example, consider these two queries:</span></span> 

    - <span data-ttu-id="de9f9-141">Wszystkie filmy z złośliwych użytkowników, których nazwisko zaczyna się od "A".</span><span class="sxs-lookup"><span data-stu-id="de9f9-141">All movies with actors whose last name starts with ‘A'.</span></span>
    - <span data-ttu-id="de9f9-142">Wszystkie filmy wydane w 1994 r.</span><span class="sxs-lookup"><span data-stu-id="de9f9-142">All movies released in 1994.</span></span>

    <span data-ttu-id="de9f9-143">Chyba, że filmy są indeksowane przez złośliwych użytkowników, pierwszego zapytania mogą wymagać aparat bazy danych, aby przeprowadzić skanowanie całą listę filmów.</span><span class="sxs-lookup"><span data-stu-id="de9f9-143">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="de9f9-144">Drugiego zapytania mogą być akceptowane, filmy przyjmuje są indeksowane według roku wydania.</span><span class="sxs-lookup"><span data-stu-id="de9f9-144">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="de9f9-145">Poniższy kod przedstawia modułu sprawdzania poprawności, który umożliwia filtrowanie właściwości "ReleaseYear" i "Title", ale ma inne właściwości.</span><span class="sxs-lookup"><span data-stu-id="de9f9-145">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="de9f9-146">Ogólnie rzecz biorąc należy wziąć pod uwagę jakie potrzebne funkcje $filter.</span><span class="sxs-lookup"><span data-stu-id="de9f9-146">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="de9f9-147">Jeśli klienci nie potrzebują pełnego wyrazistość z $filter, można ograniczyć dozwolonych funkcji.</span><span class="sxs-lookup"><span data-stu-id="de9f9-147">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
