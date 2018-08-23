---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Obsługa opcji zapytań protokołu OData, we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/04/2013
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 8745183125c9dd1dcc7cb0e146367a893bdb0170
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755074"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="6c33b-102">Obsługa opcji zapytań protokołu OData we wzorcu ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="6c33b-102">Supporting OData Query Options in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="6c33b-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6c33b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="6c33b-104">Usługa OData definiuje parametry, które mogą służyć do modyfikowania zapytanie OData.</span><span class="sxs-lookup"><span data-stu-id="6c33b-104">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="6c33b-105">Klient wysyła te parametry do ciągu zapytania identyfikatora URI żądania.</span><span class="sxs-lookup"><span data-stu-id="6c33b-105">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="6c33b-106">Na przykład aby posortować wyniki, klient używa parametru $orderby:</span><span class="sxs-lookup"><span data-stu-id="6c33b-106">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="6c33b-107">Specyfikacja protokołu OData wywołuje te parametry *opcje kwerendy*.</span><span class="sxs-lookup"><span data-stu-id="6c33b-107">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="6c33b-108">Można włączyć opcji zapytania OData dla każdego kontrolera interfejsu API sieci Web w projekcie &#8212; kontrolera musi być punktu końcowego OData.</span><span class="sxs-lookup"><span data-stu-id="6c33b-108">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="6c33b-109">Zapewnia wygodny sposób dodawania funkcji, takich jak filtrowanie i sortowanie do dowolnej aplikacji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6c33b-109">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="6c33b-110">Przed włączeniem opcji zapytań, przeczytaj temat [wskazówki dotyczące zabezpieczeń OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="6c33b-110">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="6c33b-111">Włączenie opcji zapytań protokołu OData</span><span class="sxs-lookup"><span data-stu-id="6c33b-111">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="6c33b-112">Przykładowe zapytania</span><span class="sxs-lookup"><span data-stu-id="6c33b-112">Example Queries</span></span>](#examples)
- [<span data-ttu-id="6c33b-113">Stronicowania opartych na serwerze</span><span class="sxs-lookup"><span data-stu-id="6c33b-113">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="6c33b-114">Ograniczanie opcje kwerendy</span><span class="sxs-lookup"><span data-stu-id="6c33b-114">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="6c33b-115">Bezpośrednie wywoływanie opcje kwerendy</span><span class="sxs-lookup"><span data-stu-id="6c33b-115">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="6c33b-116">Sprawdzanie poprawności zapytań</span><span class="sxs-lookup"><span data-stu-id="6c33b-116">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="6c33b-117">Włączenie opcji zapytań protokołu OData</span><span class="sxs-lookup"><span data-stu-id="6c33b-117">Enabling OData Query Options</span></span>

<span data-ttu-id="6c33b-118">Internetowy interfejs API obsługuje następujące opcje zapytania OData:</span><span class="sxs-lookup"><span data-stu-id="6c33b-118">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="6c33b-119">Opcja</span><span class="sxs-lookup"><span data-stu-id="6c33b-119">Option</span></span> | <span data-ttu-id="6c33b-120">Opis</span><span class="sxs-lookup"><span data-stu-id="6c33b-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6c33b-121">$expand</span><span class="sxs-lookup"><span data-stu-id="6c33b-121">$expand</span></span> | <span data-ttu-id="6c33b-122">Rozszerza wbudowany powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="6c33b-122">Expands related entities inline.</span></span> |
| <span data-ttu-id="6c33b-123">$filter</span><span class="sxs-lookup"><span data-stu-id="6c33b-123">$filter</span></span> | <span data-ttu-id="6c33b-124">Służy do przefiltrowania wyników, w oparciu o warunek logiczny.</span><span class="sxs-lookup"><span data-stu-id="6c33b-124">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="6c33b-125">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="6c33b-125">$inlinecount</span></span> | <span data-ttu-id="6c33b-126">Informuje serwer, które mają zostać objęte łączna liczba zgodnych jednostek odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="6c33b-126">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="6c33b-127">(Przydatne w przypadku stronicowania po stronie serwera.)</span><span class="sxs-lookup"><span data-stu-id="6c33b-127">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="6c33b-128">$orderby</span><span class="sxs-lookup"><span data-stu-id="6c33b-128">$orderby</span></span> | <span data-ttu-id="6c33b-129">Sortuje wyniki.</span><span class="sxs-lookup"><span data-stu-id="6c33b-129">Sorts the results.</span></span> |
| <span data-ttu-id="6c33b-130">$select</span><span class="sxs-lookup"><span data-stu-id="6c33b-130">$select</span></span> | <span data-ttu-id="6c33b-131">Wybiera właściwości do uwzględnienia w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="6c33b-131">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="6c33b-132">$skip</span><span class="sxs-lookup"><span data-stu-id="6c33b-132">$skip</span></span> | <span data-ttu-id="6c33b-133">Pomija pierwszych n wyników.</span><span class="sxs-lookup"><span data-stu-id="6c33b-133">Skips the first n results.</span></span> |
| <span data-ttu-id="6c33b-134">$top</span><span class="sxs-lookup"><span data-stu-id="6c33b-134">$top</span></span> | <span data-ttu-id="6c33b-135">Zwraca pierwsze n wyniki.</span><span class="sxs-lookup"><span data-stu-id="6c33b-135">Returns only the first n the results.</span></span> |

<span data-ttu-id="6c33b-136">Aby użyć opcji zapytania OData, należy je jawnie włączyć.</span><span class="sxs-lookup"><span data-stu-id="6c33b-136">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="6c33b-137">Można je włączyć ją globalnie dla całej aplikacji, lub włączyć je do określonych kontrolery lub konkretne akcje.</span><span class="sxs-lookup"><span data-stu-id="6c33b-137">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="6c33b-138">Aby włączyć ją globalnie opcje zapytania OData, należy wywołać **EnableQuerySupport** na **HttpConfiguration** klasy przy uruchamianiu:</span><span class="sxs-lookup"><span data-stu-id="6c33b-138">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="6c33b-139">**EnableQuerySupport** metoda zapewnia opcje zapytania globalnie dla każdej akcji kontrolera, które zwraca **IQueryable** typu.</span><span class="sxs-lookup"><span data-stu-id="6c33b-139">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="6c33b-140">Jeśli nie chcesz, aby opcje zapytania włączone dla całej aplikacji, można je włączyć dla akcji określonego kontrolera, dodając **[Queryable]** atrybutu do metody akcji.</span><span class="sxs-lookup"><span data-stu-id="6c33b-140">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="6c33b-141">Przykładowe zapytania</span><span class="sxs-lookup"><span data-stu-id="6c33b-141">Example Queries</span></span>

<span data-ttu-id="6c33b-142">W tej sekcji przedstawiono typów kwerend, które są możliwe przy użyciu opcji zapytania OData.</span><span class="sxs-lookup"><span data-stu-id="6c33b-142">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="6c33b-143">Aby uzyskać szczegółowe informacje na temat opcji zapytania, zapoznaj się z dokumentacją OData na [www.odata.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="6c33b-143">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="6c33b-144">W przypadku informacji na temat $Rozwiń i $select, zobacz [przy użyciu $select, $expand and $value w protokole OData składnika Web API platformy ASP.NET](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="6c33b-144">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

<span data-ttu-id="6c33b-145">**Stronicowania opartych na klienta**</span><span class="sxs-lookup"><span data-stu-id="6c33b-145">**Client-Driven Paging**</span></span>

<span data-ttu-id="6c33b-146">Dla zestawów dużych jednostek klient chcieć ograniczyć liczbę wyników.</span><span class="sxs-lookup"><span data-stu-id="6c33b-146">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="6c33b-147">Na przykład klient może wyświetlać 10 wpisów w czasie, wraz z łączami "dalej", aby uzyskać następnej strony wyników.</span><span class="sxs-lookup"><span data-stu-id="6c33b-147">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="6c33b-148">Aby to zrobić, klient używa opcji $top i $skip.</span><span class="sxs-lookup"><span data-stu-id="6c33b-148">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="6c33b-149">Opcja $top zapewnia maksymalną liczbę zwracanych pozycji, a opcja $skip zapewnia liczba wpisów do pominięcia.</span><span class="sxs-lookup"><span data-stu-id="6c33b-149">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="6c33b-150">Poprzedni przykład pobiera wpisy 21 do 30.</span><span class="sxs-lookup"><span data-stu-id="6c33b-150">The previous example fetches entries 21 through 30.</span></span>

<span data-ttu-id="6c33b-151">**Filtrowanie**</span><span class="sxs-lookup"><span data-stu-id="6c33b-151">**Filtering**</span></span>

<span data-ttu-id="6c33b-152">Opcja $filter umożliwia klientowi filtrowanie wyników za pomocą wyrażenia logicznego.</span><span class="sxs-lookup"><span data-stu-id="6c33b-152">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="6c33b-153">Wyrażenia filtru są bardzo wydajny; obejmują one operatory logiczne i arytmetyczne, parametry funkcji i funkcji daty.</span><span class="sxs-lookup"><span data-stu-id="6c33b-153">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="6c33b-154">Zwróć wszystkie produkty z kategorii jest równa "Zabawki".</span><span class="sxs-lookup"><span data-stu-id="6c33b-154">Return all products with category equal to "Toys".</span></span> | <span data-ttu-id="6c33b-155">`http://localhost/Products?$filter=Category` EQ "Zabawki"</span><span class="sxs-lookup"><span data-stu-id="6c33b-155">`http://localhost/Products?$filter=Category` eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="6c33b-156">Zwróć wszystkie produkty z ceną mniej niż 10.</span><span class="sxs-lookup"><span data-stu-id="6c33b-156">Return all products with price less than 10.</span></span> | <span data-ttu-id="6c33b-157">`http://localhost/Products?$filter=Price` lt 10</span><span class="sxs-lookup"><span data-stu-id="6c33b-157">`http://localhost/Products?$filter=Price` lt 10</span></span> |
| <span data-ttu-id="6c33b-158">Operatory logiczne: zwraca wszystkie produkty których cena > = 5, a cena < = 15.</span><span class="sxs-lookup"><span data-stu-id="6c33b-158">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | <span data-ttu-id="6c33b-159">`http://localhost/Products?$filter=Price` GE 5 i le ceny 15</span><span class="sxs-lookup"><span data-stu-id="6c33b-159">`http://localhost/Products?$filter=Price` ge 5 and Price le 15</span></span> |
| <span data-ttu-id="6c33b-160">Funkcje ciągów: zwraca wszystkie produkty z "zz" w nazwie.</span><span class="sxs-lookup"><span data-stu-id="6c33b-160">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="6c33b-161">Funkcje daty: zwraca wszystkie produkty z ReleaseDate po 2005.</span><span class="sxs-lookup"><span data-stu-id="6c33b-161">Date functions: Return all products with ReleaseDate after 2005.</span></span> | <span data-ttu-id="6c33b-162">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span><span class="sxs-lookup"><span data-stu-id="6c33b-162">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span></span> |

<span data-ttu-id="6c33b-163">**Sortowanie**</span><span class="sxs-lookup"><span data-stu-id="6c33b-163">**Sorting**</span></span>

<span data-ttu-id="6c33b-164">Aby posortować wyniki, użyj filtru $orderby.</span><span class="sxs-lookup"><span data-stu-id="6c33b-164">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="6c33b-165">Sortuj według ceny.</span><span class="sxs-lookup"><span data-stu-id="6c33b-165">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="6c33b-166">Sortuj według ceny w kolejności (od najwyższej do najniższej).</span><span class="sxs-lookup"><span data-stu-id="6c33b-166">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="6c33b-167">Sortuj według kategorii, a następnie Sortuj według ceny w porządku malejącym w kategoriach.</span><span class="sxs-lookup"><span data-stu-id="6c33b-167">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="6c33b-168">Stronicowania opartych na serwerze</span><span class="sxs-lookup"><span data-stu-id="6c33b-168">Server-Driven Paging</span></span>

<span data-ttu-id="6c33b-169">Jeśli baza danych zawiera miliony rekordów, nie chcesz wysyłać je wszystkie w jednym ładunku.</span><span class="sxs-lookup"><span data-stu-id="6c33b-169">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="6c33b-170">Aby tego uniknąć, serwer można ograniczyć liczbę wpisów, które wysyła w jednej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="6c33b-170">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="6c33b-171">Aby włączyć stronicowania serwera, należy ustawić **PageSize** właściwość **Queryable** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="6c33b-171">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="6c33b-172">Wartość jest maksymalna liczba wpisów do zwrócenia.</span><span class="sxs-lookup"><span data-stu-id="6c33b-172">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="6c33b-173">Jeśli kontroler zwraca OData format, treść odpowiedzi będzie zawierać link do następnej strony danych:</span><span class="sxs-lookup"><span data-stu-id="6c33b-173">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="6c33b-174">Klient może używać tego łącza do pobrania następnej strony.</span><span class="sxs-lookup"><span data-stu-id="6c33b-174">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="6c33b-175">Aby dowiedzieć się, łączna liczba wpisów w zestawie wyników, klienta można ustawić za pomocą wartości opcji zapytania $inlinecount "allpages".</span><span class="sxs-lookup"><span data-stu-id="6c33b-175">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="6c33b-176">Wartość "allpages" informuje serwera, które mają zostać objęte łączna liczba odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="6c33b-176">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="6c33b-177">Następna strona łącza i liczbę inlinecount wymaga formatu OData.</span><span class="sxs-lookup"><span data-stu-id="6c33b-177">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="6c33b-178">Przyczyną jest to, że usługa OData definiuje specjalne pola w treści odpowiedzi na potrzeby przechowywania łącze i liczba.</span><span class="sxs-lookup"><span data-stu-id="6c33b-178">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>


<span data-ttu-id="6c33b-179">W przypadku formatów OData bez jest nadal możliwe do obsługi liczby linków i wbudowane Następna strona opakowując wyniki zapytania w **PageResult&lt;T&gt;**  obiektu.</span><span class="sxs-lookup"><span data-stu-id="6c33b-179">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="6c33b-180">Jednak wymaga nieco więcej kodu.</span><span class="sxs-lookup"><span data-stu-id="6c33b-180">However, it requires a bit more code.</span></span> <span data-ttu-id="6c33b-181">Oto przykład:</span><span class="sxs-lookup"><span data-stu-id="6c33b-181">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="6c33b-182">Oto przykład odpowiedź w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="6c33b-182">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="6c33b-183">Ograniczanie opcje kwerendy</span><span class="sxs-lookup"><span data-stu-id="6c33b-183">Limiting the Query Options</span></span>

<span data-ttu-id="6c33b-184">Opcje zapytania zaoferować klientowi produkt szczegółową kontrolę nad zapytanie, które jest uruchamiane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6c33b-184">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="6c33b-185">W niektórych przypadkach możesz chcieć ograniczyć opcje dostępne ze względów bezpieczeństwa ani wydajności.</span><span class="sxs-lookup"><span data-stu-id="6c33b-185">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="6c33b-186">**[Queryable]** atrybut ma niektórych wbudowanych właściwości dla tego.</span><span class="sxs-lookup"><span data-stu-id="6c33b-186">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="6c33b-187">Poniżej przedstawiono kilka przykładów.</span><span class="sxs-lookup"><span data-stu-id="6c33b-187">Here are some examples.</span></span>

<span data-ttu-id="6c33b-188">Zezwalaj na tylko $skip i $top, do obsługi stronicowania i od niczego więcej:</span><span class="sxs-lookup"><span data-stu-id="6c33b-188">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="6c33b-189">Zezwalaj na porządkowanie tylko przez określonych właściwości zapobiec sortowanie według właściwości, które nie są indeksowane w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="6c33b-189">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="6c33b-190">Zezwalaj na funkcję logiczną "eq", ale inne funkcje logiczne:</span><span class="sxs-lookup"><span data-stu-id="6c33b-190">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="6c33b-191">Nie zezwalaj na wszystkie operatory arytmetyczne:</span><span class="sxs-lookup"><span data-stu-id="6c33b-191">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="6c33b-192">Opcje można ograniczyć globalnie tworząc **klasie QueryableAttribute** wystąpienia i przekazanie jej do **EnableQuerySupport** funkcji:</span><span class="sxs-lookup"><span data-stu-id="6c33b-192">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="6c33b-193">Bezpośrednie wywoływanie opcje kwerendy</span><span class="sxs-lookup"><span data-stu-id="6c33b-193">Invoking Query Options Directly</span></span>

<span data-ttu-id="6c33b-194">Zamiast używania **[Queryable]** atrybut, można wywołać opcje zapytania bezpośrednio w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="6c33b-194">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="6c33b-195">Aby to zrobić, Dodaj **ODataQueryOptions** parametr do metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6c33b-195">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="6c33b-196">W takim przypadku nie ma potrzeby **[Queryable]** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="6c33b-196">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="6c33b-197">Internetowy interfejs API wypełnia **ODataQueryOptions** ciąg zapytania z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="6c33b-197">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="6c33b-198">Stosuje zapytanie, polega na przekazaniu **IQueryable** do **ApplyTo** metody.</span><span class="sxs-lookup"><span data-stu-id="6c33b-198">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="6c33b-199">Metoda ta zwraca innego **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="6c33b-199">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="6c33b-200">W przypadku zaawansowanych scenariuszy, jeśli nie masz **IQueryable** dostawcę zapytań, można sprawdzić **ODataQueryOptions** i tłumaczyć opcje zapytania do innego formularza.</span><span class="sxs-lookup"><span data-stu-id="6c33b-200">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="6c33b-201">(Na przykład, zobacz wpis w blogu RaghuRam Nadiminti [tłumaczenie zapytań protokołu OData do HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), który zawiera również [przykładowe](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span><span class="sxs-lookup"><span data-stu-id="6c33b-201">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="6c33b-202">Sprawdzanie poprawności zapytań</span><span class="sxs-lookup"><span data-stu-id="6c33b-202">Query Validation</span></span>

<span data-ttu-id="6c33b-203">**[Queryable]** atrybut weryfikuje zapytanie przed jej wykonanie.</span><span class="sxs-lookup"><span data-stu-id="6c33b-203">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="6c33b-204">W kroku sprawdzania poprawności jest wykonywane w **QueryableAttribute.ValidateQuery** metody.</span><span class="sxs-lookup"><span data-stu-id="6c33b-204">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="6c33b-205">Można również dostosować proces sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="6c33b-205">You can also customize the validation process.</span></span>

<span data-ttu-id="6c33b-206">Zobacz też [wskazówki dotyczące zabezpieczeń OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="6c33b-206">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="6c33b-207">Najpierw zastąpienie jeden moduł weryfikacji to znaczy klas zdefiniowanych w **Web.Http.OData.Query.Validators** przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="6c33b-207">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="6c33b-208">Na przykład następujące klasy modułu sprawdzania poprawności wyłącza opcję "desc" dla opcji $orderby.</span><span class="sxs-lookup"><span data-stu-id="6c33b-208">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="6c33b-209">Podklasy **[Queryable]** atrybutu, aby zastąpić **ValidateQuery** metody.</span><span class="sxs-lookup"><span data-stu-id="6c33b-209">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="6c33b-210">Następnie ustaw niestandardowy atrybut albo globalnie lub na kontrolerze:</span><span class="sxs-lookup"><span data-stu-id="6c33b-210">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="6c33b-211">Jeśli używasz **ODataQueryOptions** bezpośrednio, ustaw modułu sprawdzania poprawności na temat opcji:</span><span class="sxs-lookup"><span data-stu-id="6c33b-211">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
