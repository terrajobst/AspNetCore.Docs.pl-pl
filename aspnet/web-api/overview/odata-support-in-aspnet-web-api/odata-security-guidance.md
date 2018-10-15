---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Wskazówki dotyczące zabezpieczeń dla wzorca ASP.NET Web API 2 OData | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/06/2013
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 4ba53e15dab83368097a58ba4d0d2e46d113d1d2
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325721"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="56ebc-102">Wskazówki dotyczące zabezpieczeń dla wzorca ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="56ebc-102">Security Guidance for ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="56ebc-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="56ebc-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="56ebc-104">W tym temacie opisano niektóre problemy z zabezpieczeniami, które należy wziąć pod uwagę podczas udostępniania zestawu danych za pośrednictwem interfejsu OData.</span><span class="sxs-lookup"><span data-stu-id="56ebc-104">This topic describes some of the security issues that you should consider when exposing a dataset through OData.</span></span>

## <a name="edm-security"></a><span data-ttu-id="56ebc-105">Zabezpieczenia EDM</span><span class="sxs-lookup"><span data-stu-id="56ebc-105">EDM Security</span></span>

<span data-ttu-id="56ebc-106">Semantyki zapytań są oparte na modelu entity data model (EDM) struktury, nie podstawowych typów modelu.</span><span class="sxs-lookup"><span data-stu-id="56ebc-106">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="56ebc-107">Właściwości można wykluczyć z EDM i nie będą widoczne dla zapytania.</span><span class="sxs-lookup"><span data-stu-id="56ebc-107">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="56ebc-108">Na przykład załóżmy, że model zawiera typ pracownika z właściwością wynagrodzenia.</span><span class="sxs-lookup"><span data-stu-id="56ebc-108">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="56ebc-109">Możesz chcieć wyłączyć tę właściwość z EDM, aby go ukryć od klientów.</span><span class="sxs-lookup"><span data-stu-id="56ebc-109">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="56ebc-110">Istnieją dwa sposoby wyłączają właściwość EDM.</span><span class="sxs-lookup"><span data-stu-id="56ebc-110">There are two ways to exlude a property from the EDM.</span></span> <span data-ttu-id="56ebc-111">Możesz ustawić **[IgnoreDataMember]** atrybutu dla właściwości w klasie modelu:</span><span class="sxs-lookup"><span data-stu-id="56ebc-111">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="56ebc-112">Możesz również usunąć właściwość z EDM programowe:</span><span class="sxs-lookup"><span data-stu-id="56ebc-112">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="56ebc-113">Zabezpieczenie zapytania</span><span class="sxs-lookup"><span data-stu-id="56ebc-113">Query Security</span></span>

<span data-ttu-id="56ebc-114">Złośliwego lub prostego klienta można utworzyć zapytanie, które bardzo długi czas do wykonania.</span><span class="sxs-lookup"><span data-stu-id="56ebc-114">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="56ebc-115">W najgorszym przypadku może to zakłócać dostęp do usługi.</span><span class="sxs-lookup"><span data-stu-id="56ebc-115">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="56ebc-116">**[Queryable]** atrybut jest filtr akcji, która analizuje, weryfikuje i stosuje zapytanie.</span><span class="sxs-lookup"><span data-stu-id="56ebc-116">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="56ebc-117">Filtr konwertuje opcje zapytania w wyrażeniu LINQ.</span><span class="sxs-lookup"><span data-stu-id="56ebc-117">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="56ebc-118">Gdy zwraca kontrolera OData **IQueryable** typu **IQueryable** dostawcy LINQ konwertuje wyrażenie LINQ do kwerendy.</span><span class="sxs-lookup"><span data-stu-id="56ebc-118">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="56ebc-119">W związku z tym wydajność zależy od dostawcy LINQ, która jest używana, a także na konkretnej właściwości schemat zestawu danych lub bazy danych.</span><span class="sxs-lookup"><span data-stu-id="56ebc-119">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="56ebc-120">Aby uzyskać więcej informacji o używaniu opcji zapytania protokołu OData w interfejsie API sieci Web platformy ASP.NET, zobacz [obsługi opcji zapytania OData](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="56ebc-120">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="56ebc-121">Jeśli wiesz, że wszyscy klienci są zaufane (na przykład w środowisku przedsiębiorstwa) lub zestaw danych jest małe, wydajność zapytań nie może być problem.</span><span class="sxs-lookup"><span data-stu-id="56ebc-121">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="56ebc-122">W przeciwnym razie należy rozważyć poniższe zalecenia.</span><span class="sxs-lookup"><span data-stu-id="56ebc-122">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="56ebc-123">Testowanie usługi z różnych zapytań i profilowanie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="56ebc-123">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="56ebc-124">Włączanie stronicowania opartych na serwerze uniknąć zwrócenie dużych zestawów danych w jednym zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="56ebc-124">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="56ebc-125">Aby uzyskać więcej informacji, zobacz [stronicowania Server-Driven](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="56ebc-125">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="56ebc-126">Potrzebujesz $filter i $orderby?</span><span class="sxs-lookup"><span data-stu-id="56ebc-126">Do you need $filter and $orderby?</span></span> <span data-ttu-id="56ebc-127">Niektóre aplikacje mogą zezwolić na klientów, stronicowania, przy użyciu $top i $skip, ale wyłącz innymi opcjami zapytania.</span><span class="sxs-lookup"><span data-stu-id="56ebc-127">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="56ebc-128">Należy rozważyć ograniczenie $orderby do właściwości w indeksie klastrowanym.</span><span class="sxs-lookup"><span data-stu-id="56ebc-128">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="56ebc-129">Sortowanie dużej ilości danych bez indeksu klastrowanego trwa długo.</span><span class="sxs-lookup"><span data-stu-id="56ebc-129">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="56ebc-130">Maksymalna dopuszczalna liczba węzłów: **MaxNodeCount** właściwość **[Queryable]** ustawia maksymalnej liczby węzłów w drzewie składni $filter dozwolone.</span><span class="sxs-lookup"><span data-stu-id="56ebc-130">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="56ebc-131">Wartość domyślna to 100, ale warto ustawić niższą wartość, ponieważ dużą liczbę węzłów może być powolne skompilować.</span><span class="sxs-lookup"><span data-stu-id="56ebc-131">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="56ebc-132">Jest to szczególnie istotne w przypadku korzystania z LINQ to Objects (czyli zapytań LINQ w kolekcji w pamięci, bez użycia pośrednie dostawcy LINQ).</span><span class="sxs-lookup"><span data-stu-id="56ebc-132">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="56ebc-133">Rozważ wyłączenie funkcji funkcja any() i all(), ponieważ mogą one być powolne.</span><span class="sxs-lookup"><span data-stu-id="56ebc-133">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="56ebc-134">Jeśli wszystkie właściwości parametrów zawierają dużych ciągów&#8212;na przykład opis produktu lub wpis w blogu&#8212;Rozważ wyłączenie funkcji ciągów.</span><span class="sxs-lookup"><span data-stu-id="56ebc-134">If any string properties contain large strings&#8212;for example, a product description or a blog entry&#8212;consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="56ebc-135">Należy wziąć pod uwagę, nie można przydzielać filtrowania według właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="56ebc-135">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="56ebc-136">Filtrowanie według właściwości nawigacji może spowodować sprzężenie, która może być powolne, w zależności od schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="56ebc-136">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="56ebc-137">Poniższy kod przedstawia moduł weryfikacji zapytania, który uniemożliwia filtrowania według właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="56ebc-137">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="56ebc-138">Aby uzyskać więcej informacji na temat modułów weryfikacji zapytań zobacz [sprawdzanie poprawności zapytań](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="56ebc-138">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="56ebc-139">Należy rozważyć ograniczenie zapytania $filter, pisząc modułu sprawdzania poprawności, który jest dostosowany do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="56ebc-139">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="56ebc-140">Na przykład należy wziąć pod uwagę te dwa zapytania:</span><span class="sxs-lookup"><span data-stu-id="56ebc-140">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="56ebc-141">Wszystkie filmy z aktorów, których nazwisko zaczyna się od "A".</span><span class="sxs-lookup"><span data-stu-id="56ebc-141">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="56ebc-142">Wydane w 1994 r. wszystkie filmy.</span><span class="sxs-lookup"><span data-stu-id="56ebc-142">All movies released in 1994.</span></span>

    <span data-ttu-id="56ebc-143">Chyba że filmy są indeksowane przez podmioty, pierwsze zapytanie może wymagać aparat bazy danych w celu skanowania całą listę filmów.</span><span class="sxs-lookup"><span data-stu-id="56ebc-143">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="56ebc-144">Drugie zapytanie może być akceptowalne, przyjmuje filmy są indeksowane przez rok wydania.</span><span class="sxs-lookup"><span data-stu-id="56ebc-144">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="56ebc-145">Poniższy kod przedstawia modułu sprawdzania poprawności, który umożliwia filtrowanie na właściwości "ReleaseYear" i "Title", ale żadne inne właściwości.</span><span class="sxs-lookup"><span data-stu-id="56ebc-145">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="56ebc-146">Ogólnie rzecz biorąc należy wziąć pod uwagę jakie funkcje $filter, potrzebujesz.</span><span class="sxs-lookup"><span data-stu-id="56ebc-146">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="56ebc-147">Jeśli klienci pełną wyrazistość $filter nie jest konieczne, można ograniczyć dozwolonych funkcji.</span><span class="sxs-lookup"><span data-stu-id="56ebc-147">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
