---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: "Obsługa opcje zapytania OData w składniku ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 004c029db6f01627f7cadff26aaf5554ce2b93a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="f973f-102">Obsługa opcje zapytania OData w składniku ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="f973f-102">Supporting OData Query Options in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="f973f-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f973f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f973f-104">OData definiuje parametry, które mogą służyć do modyfikowania zapytania OData.</span><span class="sxs-lookup"><span data-stu-id="f973f-104">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="f973f-105">Klient wysyła te parametry w ciągu zapytania identyfikatora URI żądania.</span><span class="sxs-lookup"><span data-stu-id="f973f-105">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="f973f-106">Na przykład aby sortować wyniki, klient używa parametru $orderby:</span><span class="sxs-lookup"><span data-stu-id="f973f-106">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="f973f-107">Specyfikację OData wywołuje te parametry *opcje kwerendy*.</span><span class="sxs-lookup"><span data-stu-id="f973f-107">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="f973f-108">Można włączyć opcji zapytania OData dla każdego kontrolera interfejsu API sieci Web w projekcie &#8212; kontroler nie musi być punkt końcowy OData.</span><span class="sxs-lookup"><span data-stu-id="f973f-108">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="f973f-109">Zapewnia to wygodny sposób, aby dodać funkcje, takie jak filtrowanie i sortowanie do dowolnej aplikacji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f973f-109">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="f973f-110">Przed włączeniem opcje zapytania, przeczytaj temat [wskazówki dotyczące zabezpieczeń OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="f973f-110">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="f973f-111">Włączanie opcji zapytania OData</span><span class="sxs-lookup"><span data-stu-id="f973f-111">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="f973f-112">Przykładowe zapytania</span><span class="sxs-lookup"><span data-stu-id="f973f-112">Example Queries</span></span>](#examples)
- [<span data-ttu-id="f973f-113">Stronicowania obsługiwanego przez serwer</span><span class="sxs-lookup"><span data-stu-id="f973f-113">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="f973f-114">Ograniczanie opcje zapytania</span><span class="sxs-lookup"><span data-stu-id="f973f-114">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="f973f-115">Bezpośrednie wywoływanie opcje zapytania</span><span class="sxs-lookup"><span data-stu-id="f973f-115">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="f973f-116">Sprawdzanie poprawności zapytań</span><span class="sxs-lookup"><span data-stu-id="f973f-116">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="f973f-117">Włączanie opcji zapytania OData</span><span class="sxs-lookup"><span data-stu-id="f973f-117">Enabling OData Query Options</span></span>

<span data-ttu-id="f973f-118">Interfejs API sieci Web obsługuje następujące opcje zapytania OData:</span><span class="sxs-lookup"><span data-stu-id="f973f-118">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="f973f-119">Opcja</span><span class="sxs-lookup"><span data-stu-id="f973f-119">Option</span></span> | <span data-ttu-id="f973f-120">Opis</span><span class="sxs-lookup"><span data-stu-id="f973f-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f973f-121">Rozwiń węzeł $</span><span class="sxs-lookup"><span data-stu-id="f973f-121">$expand</span></span> | <span data-ttu-id="f973f-122">Rozwija wbudowanego powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="f973f-122">Expands related entities inline.</span></span> |
| <span data-ttu-id="f973f-123">$filter</span><span class="sxs-lookup"><span data-stu-id="f973f-123">$filter</span></span> | <span data-ttu-id="f973f-124">Filtruje wyniki, w oparciu o warunek typu Boolean.</span><span class="sxs-lookup"><span data-stu-id="f973f-124">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="f973f-125">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="f973f-125">$inlinecount</span></span> | <span data-ttu-id="f973f-126">Określa, że serwer do uwzględnienia w odpowiedzi łączna liczba zgodnych jednostek.</span><span class="sxs-lookup"><span data-stu-id="f973f-126">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="f973f-127">(Przydatne w przypadku stronicowania po stronie serwera.)</span><span class="sxs-lookup"><span data-stu-id="f973f-127">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="f973f-128">$orderby</span><span class="sxs-lookup"><span data-stu-id="f973f-128">$orderby</span></span> | <span data-ttu-id="f973f-129">Wyniki są sortowane.</span><span class="sxs-lookup"><span data-stu-id="f973f-129">Sorts the results.</span></span> |
| <span data-ttu-id="f973f-130">$select</span><span class="sxs-lookup"><span data-stu-id="f973f-130">$select</span></span> | <span data-ttu-id="f973f-131">Wybiera właściwości, które do uwzględnienia w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f973f-131">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="f973f-132">$skip</span><span class="sxs-lookup"><span data-stu-id="f973f-132">$skip</span></span> | <span data-ttu-id="f973f-133">Pomija pierwsze n wyniki.</span><span class="sxs-lookup"><span data-stu-id="f973f-133">Skips the first n results.</span></span> |
| <span data-ttu-id="f973f-134">$top</span><span class="sxs-lookup"><span data-stu-id="f973f-134">$top</span></span> | <span data-ttu-id="f973f-135">Zwraca pierwsze n wyniki.</span><span class="sxs-lookup"><span data-stu-id="f973f-135">Returns only the first n the results.</span></span> |

<span data-ttu-id="f973f-136">Aby użyć opcji zapytania OData, należy je jawnie włączyć.</span><span class="sxs-lookup"><span data-stu-id="f973f-136">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="f973f-137">Możesz je włączyć globalnie dla całej aplikacji lub włączyć je do określonych kontrolerów lub określonych akcji.</span><span class="sxs-lookup"><span data-stu-id="f973f-137">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="f973f-138">Aby włączyć globalnie opcje zapytania OData, należy wywołać **EnableQuerySupport** na **HttpConfiguration** klasy podczas uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="f973f-138">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="f973f-139">**EnableQuerySupport** metoda zapewnia opcje zapytania globalnie dla każdej akcji kontrolera, która zwraca **IQueryable** typu.</span><span class="sxs-lookup"><span data-stu-id="f973f-139">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="f973f-140">Jeśli nie chcesz, aby opcje zapytania włączony dla całej aplikacji, można włączyć je do akcji kontrolera określonej przez dodanie **[Queryable]** atrybut do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="f973f-140">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="f973f-141">Przykładowe zapytania</span><span class="sxs-lookup"><span data-stu-id="f973f-141">Example Queries</span></span>

<span data-ttu-id="f973f-142">W tej sekcji przedstawiono typy zapytań, które są możliwe przy użyciu opcji zapytania OData.</span><span class="sxs-lookup"><span data-stu-id="f973f-142">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="f973f-143">Aby uzyskać szczegółowe informacje na temat opcji zapytania, zapoznaj się z dokumentacją OData, w [www.odata.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="f973f-143">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="f973f-144">Informacje o $Rozwiń i $select, zobacz [przy użyciu $select, rozwinąć $ i $value w programie ASP.NET Web API OData](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="f973f-144">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

<span data-ttu-id="f973f-145">**Stronicowania obsługiwanego przez klienta**</span><span class="sxs-lookup"><span data-stu-id="f973f-145">**Client-Driven Paging**</span></span>

<span data-ttu-id="f973f-146">Dla zestawów duża jednostka klienta mogą chcieć ograniczyć liczbę wyników.</span><span class="sxs-lookup"><span data-stu-id="f973f-146">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="f973f-147">Na przykład klienta mogą być wyświetlane wpisy 10 jednocześnie, wraz z łączami "dalej" do pobrania następnej strony wyników.</span><span class="sxs-lookup"><span data-stu-id="f973f-147">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="f973f-148">Aby to zrobić, klient używa opcji $top i $skip.</span><span class="sxs-lookup"><span data-stu-id="f973f-148">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="f973f-149">Opcja $top zapewnia maksymalna liczba wpisów do zwrócenia, a opcja $skip zapewnia liczba wpisów, aby pominąć.</span><span class="sxs-lookup"><span data-stu-id="f973f-149">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="f973f-150">Poprzedni przykład pobiera wpisy 21 do 30.</span><span class="sxs-lookup"><span data-stu-id="f973f-150">The previous example fetches entries 21 through 30.</span></span>

<span data-ttu-id="f973f-151">**Filtrowanie**</span><span class="sxs-lookup"><span data-stu-id="f973f-151">**Filtering**</span></span>

<span data-ttu-id="f973f-152">Opcji $filter umożliwia klientowi filtrować wyniki, stosując wyrażenie logiczne.</span><span class="sxs-lookup"><span data-stu-id="f973f-152">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="f973f-153">Wyrażenia filtru są bardzo zaawansowane; obejmują one operatorów logicznych i arytmetyczne, ciąg funkcji i funkcji daty.</span><span class="sxs-lookup"><span data-stu-id="f973f-153">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="f973f-154">Zwróć wszystkie produkty z kategorii jest równa "Toys".</span><span class="sxs-lookup"><span data-stu-id="f973f-154">Return all products with category equal to "Toys".</span></span> | <span data-ttu-id="f973f-155">`http://localhost/Products?$filter=Category`EQ "Toys"</span><span class="sxs-lookup"><span data-stu-id="f973f-155">`http://localhost/Products?$filter=Category` eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="f973f-156">Zwróć wszystkie produkty z cen mniej niż 10.</span><span class="sxs-lookup"><span data-stu-id="f973f-156">Return all products with price less than 10.</span></span> | <span data-ttu-id="f973f-157">`http://localhost/Products?$filter=Price`lt 10</span><span class="sxs-lookup"><span data-stu-id="f973f-157">`http://localhost/Products?$filter=Price` lt 10</span></span> |
| <span data-ttu-id="f973f-158">Operatory logiczne: zwraca wszystkie produkty gdzie ceny > = 5 i cen < = 15.</span><span class="sxs-lookup"><span data-stu-id="f973f-158">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | <span data-ttu-id="f973f-159">`http://localhost/Products?$filter=Price`GE 5 i cen le 15</span><span class="sxs-lookup"><span data-stu-id="f973f-159">`http://localhost/Products?$filter=Price` ge 5 and Price le 15</span></span> |
| <span data-ttu-id="f973f-160">Funkcje ciągów: zwraca wszystkie produkty z "zz" w nazwie.</span><span class="sxs-lookup"><span data-stu-id="f973f-160">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="f973f-161">Funkcje daty: zwraca wszystkie produkty z ReleaseDate po 2005.</span><span class="sxs-lookup"><span data-stu-id="f973f-161">Date functions: Return all products with ReleaseDate after 2005.</span></span> | <span data-ttu-id="f973f-162">`http://localhost/Products?$filter=year(ReleaseDate)`gt 2005</span><span class="sxs-lookup"><span data-stu-id="f973f-162">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span></span> |

<span data-ttu-id="f973f-163">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="f973f-163">**Sorting**</span></span>

<span data-ttu-id="f973f-164">Sortowanie wyników, użyj filtru $orderby.</span><span class="sxs-lookup"><span data-stu-id="f973f-164">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="f973f-165">Sortuj według ceny.</span><span class="sxs-lookup"><span data-stu-id="f973f-165">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="f973f-166">Sortuj według cen w porządku malejącym (najwyższy malejąco).</span><span class="sxs-lookup"><span data-stu-id="f973f-166">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="f973f-167">Sortuj według kategorii, a następnie sortować cen malejąco według kategorii.</span><span class="sxs-lookup"><span data-stu-id="f973f-167">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="f973f-168">Stronicowania obsługiwanego przez serwer</span><span class="sxs-lookup"><span data-stu-id="f973f-168">Server-Driven Paging</span></span>

<span data-ttu-id="f973f-169">Jeśli baza danych zawiera miliony rekordów, nie chcesz wysłać ich wszystkich w jednym ładunku.</span><span class="sxs-lookup"><span data-stu-id="f973f-169">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="f973f-170">Aby tego uniknąć, serwer może ograniczyć liczbę wpisów, które wysyła w pojedynczą odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="f973f-170">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="f973f-171">Aby włączyć stronicowania na serwerze, należy ustawić **PageSize** właściwości w **Queryable** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f973f-171">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="f973f-172">Wartość jest maksymalna liczba wpisów do zwrócenia.</span><span class="sxs-lookup"><span data-stu-id="f973f-172">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="f973f-173">Jeśli kontroler zwraca OData format, treść odpowiedzi będzie zawierać łącze do następnej strony danych:</span><span class="sxs-lookup"><span data-stu-id="f973f-173">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="f973f-174">Klient może używać to łącze do pobrania następnej strony.</span><span class="sxs-lookup"><span data-stu-id="f973f-174">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="f973f-175">Aby dowiedzieć się liczba wpisów w zestawie wyników, klienta można ustawić o wartości opcji zapytania $inlinecount "allpages".</span><span class="sxs-lookup"><span data-stu-id="f973f-175">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="f973f-176">Wartość "allpages" informuje serwer do uwzględnienia w odpowiedzi łączna liczba:</span><span class="sxs-lookup"><span data-stu-id="f973f-176">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="f973f-177">Łącza do następnej strony i liczbę inlinecount wymaga OData.</span><span class="sxs-lookup"><span data-stu-id="f973f-177">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="f973f-178">Przyczyną jest to, że OData definiuje specjalne pola w treści odpowiedzi, aby pomieścić link i liczba.</span><span class="sxs-lookup"><span data-stu-id="f973f-178">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>


<span data-ttu-id="f973f-179">W przypadku formatów OData z systemem innym niż jest nadal możliwe do obsługi liczby łącza i wbudowanego następnej strony zawijania wyniki zapytania w **element PageResult&lt;T&gt;**  obiektu.</span><span class="sxs-lookup"><span data-stu-id="f973f-179">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="f973f-180">Jednak wymaga nieco więcej kodu.</span><span class="sxs-lookup"><span data-stu-id="f973f-180">However, it requires a bit more code.</span></span> <span data-ttu-id="f973f-181">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="f973f-181">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="f973f-182">Oto przykład JSON odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="f973f-182">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="f973f-183">Ograniczanie opcje zapytania</span><span class="sxs-lookup"><span data-stu-id="f973f-183">Limiting the Query Options</span></span>

<span data-ttu-id="f973f-184">Opcje zapytania nadaj klienta dużo kontrolę nad zapytania, który jest uruchamiany na serwerze.</span><span class="sxs-lookup"><span data-stu-id="f973f-184">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="f973f-185">W niektórych przypadkach można ograniczyć dostępne opcje ze względów bezpieczeństwa ani wydajności.</span><span class="sxs-lookup"><span data-stu-id="f973f-185">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="f973f-186">**[Queryable]** atrybutu są niektóre wbudowane właściwości dla tego.</span><span class="sxs-lookup"><span data-stu-id="f973f-186">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="f973f-187">Oto kilka przykładów.</span><span class="sxs-lookup"><span data-stu-id="f973f-187">Here are some examples.</span></span>

<span data-ttu-id="f973f-188">Zezwalaj tylko $skip ani $top, do obsługi stronicowania i nic innego:</span><span class="sxs-lookup"><span data-stu-id="f973f-188">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="f973f-189">Kolejność tylko przez niektóre właściwości zapobiec sortowania dla właściwości, które nie są indeksowane w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="f973f-189">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="f973f-190">Zezwalaj na funkcję logicznych "eq", ale inne funkcje logiczne:</span><span class="sxs-lookup"><span data-stu-id="f973f-190">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="f973f-191">Nie zezwalaj na wszystkie operatory arytmetyczne:</span><span class="sxs-lookup"><span data-stu-id="f973f-191">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="f973f-192">Można ograniczyć opcje globalnie tworząc **klasie QueryableAttribute** wystąpienia i przekazanie jej do **EnableQuerySupport** funkcji:</span><span class="sxs-lookup"><span data-stu-id="f973f-192">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="f973f-193">Bezpośrednie wywoływanie opcje zapytania</span><span class="sxs-lookup"><span data-stu-id="f973f-193">Invoking Query Options Directly</span></span>

<span data-ttu-id="f973f-194">Zamiast **[Queryable]** atrybutu, opcji zapytania można wywoływać bezpośrednio w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="f973f-194">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="f973f-195">Aby to zrobić, należy dodać **ODataQueryOptions** parametru do metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f973f-195">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="f973f-196">W takim przypadku nie trzeba **[Queryable]** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f973f-196">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="f973f-197">Interfejs API sieci Web wypełnia **ODataQueryOptions** z identyfikatora URI z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="f973f-197">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="f973f-198">Aby zastosować zapytanie, należy przekazać **IQueryable** do **metody ApplyTo** metody.</span><span class="sxs-lookup"><span data-stu-id="f973f-198">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="f973f-199">Metoda zwraca innego **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="f973f-199">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="f973f-200">Dla zaawansowanych scenariuszy, jeśli nie masz **IQueryable** zapytanie do dostawcy, można sprawdzić **ODataQueryOptions** i odpowiednie opcje zapytania do innego formularza.</span><span class="sxs-lookup"><span data-stu-id="f973f-200">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="f973f-201">(Na przykład, zobacz RaghuRam Nadiminti blogu [zapytań tłumaczenia OData do HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), który obejmuje również [próbki](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span><span class="sxs-lookup"><span data-stu-id="f973f-201">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="f973f-202">Sprawdzanie poprawności zapytań</span><span class="sxs-lookup"><span data-stu-id="f973f-202">Query Validation</span></span>

<span data-ttu-id="f973f-203">**[Queryable]** atrybutu weryfikuje zapytanie przed jej wykonanie.</span><span class="sxs-lookup"><span data-stu-id="f973f-203">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="f973f-204">Krok sprawdzania poprawności jest wykonywane w **QueryableAttribute.ValidateQuery** metody.</span><span class="sxs-lookup"><span data-stu-id="f973f-204">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="f973f-205">Można również dostosować procesu weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="f973f-205">You can also customize the validation process.</span></span>

<span data-ttu-id="f973f-206">Zobacz też [wskazówki dotyczące zabezpieczeń OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="f973f-206">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="f973f-207">Najpierw zastąpienie jednego modułu sprawdzania poprawności klasy czyli zdefiniowane w **Web.Http.OData.Query.Validators** przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="f973f-207">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="f973f-208">Na przykład następujące klasy modułu sprawdzania poprawności wyłącza opcję "opis" dla opcji $orderby.</span><span class="sxs-lookup"><span data-stu-id="f973f-208">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="f973f-209">Podklasy **[Queryable]** atrybutu, aby zastąpić **ValidateQuery** metody.</span><span class="sxs-lookup"><span data-stu-id="f973f-209">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="f973f-210">Następnie ustaw atrybut niestandardowy albo globalnie lub na kontrolerze:</span><span class="sxs-lookup"><span data-stu-id="f973f-210">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="f973f-211">Jeśli używasz **ODataQueryOptions** bezpośrednio, ustaw modułu sprawdzania poprawności opcji:</span><span class="sxs-lookup"><span data-stu-id="f973f-211">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
