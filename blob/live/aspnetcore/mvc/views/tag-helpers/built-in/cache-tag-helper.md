---
title: "Buforuj pomocnika tagów w podstawowej platformy ASP.NET MVC"
author: pkellner
description: "Pokazuje, jak pracować z pamięci podręcznej pomocnika tagów"
keywords: "Platformy ASP.NET Core pomocnika tagów"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a012
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: da5b7b3bf1aa01ee22edf9bd003d8f79a00a5d0b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="df28a-104">Buforuj pomocnika tagów w podstawowej platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="df28a-104">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="df28a-105">Przez [Kellner Peterowi](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="df28a-105">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="df28a-106">Pamięć podręczna pomocnika tagów umożliwia znacznie zwiększyć wydajność aplikacji platformy ASP.NET Core buforując zawartość wewnętrzna dostawcy platformy ASP.NET Core w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="df28a-106">The  Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="df28a-107">Domyślnie ustawia aparat widoku Razor `expires-after` do 20 minut.</span><span class="sxs-lookup"><span data-stu-id="df28a-107">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="df28a-108">Następujący kod Razor buforuje daty/godziny:</span><span class="sxs-lookup"><span data-stu-id="df28a-108">The following Razor markup caches the date/time:</span></span>

```cshtml
<Cache>@DateTime.Now<Cache>
```

<span data-ttu-id="df28a-109">Pierwsze żądanie do strony zawierającej `CacheTagHelper` wyświetli bieżącej daty/godziny.</span><span class="sxs-lookup"><span data-stu-id="df28a-109">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="df28a-110">Dodatkowe żądania zostaną wyświetlone wartość w pamięci podręcznej, dopóki bufor wygasa (domyślnie 20 minut) lub jest wykluczony przez wykorzystania pamięci.</span><span class="sxs-lookup"><span data-stu-id="df28a-110">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="df28a-111">Można ustawić czas trwania pamięci podręcznej z następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="df28a-111">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="df28a-112">Buforuj atrybuty pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="df28a-112">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="df28a-113">włączone</span><span class="sxs-lookup"><span data-stu-id="df28a-113">enabled</span></span>    


| <span data-ttu-id="df28a-114">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df28a-114">Attribute Type</span></span>    | <span data-ttu-id="df28a-115">Prawidłowe wartości</span><span class="sxs-lookup"><span data-stu-id="df28a-115">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="df28a-116">wartość logiczna</span><span class="sxs-lookup"><span data-stu-id="df28a-116">boolean</span></span>           | <span data-ttu-id="df28a-117">wartość "prawda" (ustawienie domyślne)</span><span class="sxs-lookup"><span data-stu-id="df28a-117">"true" (default)</span></span>  |
|                   | <span data-ttu-id="df28a-118">"false"</span><span class="sxs-lookup"><span data-stu-id="df28a-118">"false"</span></span>   |


<span data-ttu-id="df28a-119">Określa, czy zawartość ujęty w pamięci podręcznej pomocnika tagów są buforowane.</span><span class="sxs-lookup"><span data-stu-id="df28a-119">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="df28a-120">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="df28a-120">The default is `true`.</span></span>  <span data-ttu-id="df28a-121">Jeśli ustawiono `false` tego pomocnika tagów pamięci podręcznej nie wpłyną buforowania na renderowanych danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="df28a-121">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="df28a-122">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df28a-122">Example:</span></span>

```cshtml
<Cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="df28a-123">wygasa</span><span class="sxs-lookup"><span data-stu-id="df28a-123">expires-on</span></span> 

| <span data-ttu-id="df28a-124">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df28a-124">Attribute Type</span></span>    | <span data-ttu-id="df28a-125">Przykładowa wartość</span><span class="sxs-lookup"><span data-stu-id="df28a-125">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="df28a-126">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="df28a-126">DateTimeOffset</span></span>    | <span data-ttu-id="df28a-127">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="df28a-127">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="df28a-128">Ustawia datę wygaśnięcia bezwzględne.</span><span class="sxs-lookup"><span data-stu-id="df28a-128">Sets an absolute expiration date.</span></span> <span data-ttu-id="df28a-129">Poniższy przykład będą buforowane zawartość pamięci podręcznej pomocnika tagów do 5:02 PM 29 stycznia 2025.</span><span class="sxs-lookup"><span data-stu-id="df28a-129">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="df28a-130">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df28a-130">Example:</span></span>

```cshtml
<Cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="df28a-131">Po wygaśnięciu</span><span class="sxs-lookup"><span data-stu-id="df28a-131">expires-after</span></span>

| <span data-ttu-id="df28a-132">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df28a-132">Attribute Type</span></span>    | <span data-ttu-id="df28a-133">Przykładowa wartość</span><span class="sxs-lookup"><span data-stu-id="df28a-133">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="df28a-134">Zakres czasu</span><span class="sxs-lookup"><span data-stu-id="df28a-134">TimeSpan</span></span>    | <span data-ttu-id="df28a-135">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="df28a-135">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="df28a-136">Ustawia czas od pierwszego żądania, aby buforować zawartość.</span><span class="sxs-lookup"><span data-stu-id="df28a-136">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="df28a-137">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df28a-137">Example:</span></span>

```cshtml
<Cache expires-on="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="df28a-138">Przedłużanie wygasa</span><span class="sxs-lookup"><span data-stu-id="df28a-138">expires-sliding</span></span>

| <span data-ttu-id="df28a-139">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df28a-139">Attribute Type</span></span>    | <span data-ttu-id="df28a-140">Przykładowa wartość</span><span class="sxs-lookup"><span data-stu-id="df28a-140">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="df28a-141">Zakres czasu</span><span class="sxs-lookup"><span data-stu-id="df28a-141">TimeSpan</span></span>    | <span data-ttu-id="df28a-142">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="df28a-142">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="df28a-143">Ustawia czas, który wykluczyć wpisu pamięci podręcznej, jeśli nie uzyska dostępu.</span><span class="sxs-lookup"><span data-stu-id="df28a-143">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="df28a-144">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df28a-144">Example:</span></span>

```cshtml
<Cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="df28a-145">różnią się w nagłówku</span><span class="sxs-lookup"><span data-stu-id="df28a-145">vary-by-header</span></span>

| <span data-ttu-id="df28a-146">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df28a-146">Attribute Type</span></span>    | <span data-ttu-id="df28a-147">Przykładowe wartości</span><span class="sxs-lookup"><span data-stu-id="df28a-147">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="df28a-148">String</span><span class="sxs-lookup"><span data-stu-id="df28a-148">String</span></span>            | <span data-ttu-id="df28a-149">"Agent użytkownika"</span><span class="sxs-lookup"><span data-stu-id="df28a-149">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="df28a-150">"Agent użytkownika, kodowania zawartości"</span><span class="sxs-lookup"><span data-stu-id="df28a-150">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="df28a-151">Akceptuje wartości jeden nagłówek lub rozdzielaną przecinkami listę wartości nagłówka, które mogą powodować odświeżenie pamięci podręcznej po zmianie.</span><span class="sxs-lookup"><span data-stu-id="df28a-151">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="df28a-152">Poniższy przykład monitoruje wartość nagłówka `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="df28a-152">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="df28a-153">Przykład będzie buforowanie zawartości dla każdego innego `User-Agent` przedstawiony na serwerze sieci web.</span><span class="sxs-lookup"><span data-stu-id="df28a-153">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="df28a-154">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df28a-154">Example:</span></span>

```cshtml
<Cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="df28a-155">różnią się przez zapytanie</span><span class="sxs-lookup"><span data-stu-id="df28a-155">vary-by-query</span></span>

| <span data-ttu-id="df28a-156">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df28a-156">Attribute Type</span></span>    | <span data-ttu-id="df28a-157">Przykładowe wartości</span><span class="sxs-lookup"><span data-stu-id="df28a-157">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="df28a-158">String</span><span class="sxs-lookup"><span data-stu-id="df28a-158">String</span></span>            | <span data-ttu-id="df28a-159">"Przechowuj"</span><span class="sxs-lookup"><span data-stu-id="df28a-159">"Make"</span></span>                |
|                   | <span data-ttu-id="df28a-160">"Marki, modelu"</span><span class="sxs-lookup"><span data-stu-id="df28a-160">"Make,Model"</span></span> |

<span data-ttu-id="df28a-161">Akceptuje wartości jeden nagłówek lub rozdzielaną przecinkami listę wartości nagłówka, które mogą powodować odświeżenie pamięci podręcznej po zmianie wartości nagłówka.</span><span class="sxs-lookup"><span data-stu-id="df28a-161">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="df28a-162">Poniższy przykład analizuje wartości `Make` i `Model`.</span><span class="sxs-lookup"><span data-stu-id="df28a-162">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="df28a-163">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df28a-163">Example:</span></span>

```cshtml
<Cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="df28a-164">różnią się przez trasy</span><span class="sxs-lookup"><span data-stu-id="df28a-164">vary-by-route</span></span>

| <span data-ttu-id="df28a-165">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df28a-165">Attribute Type</span></span>    | <span data-ttu-id="df28a-166">Przykładowe wartości</span><span class="sxs-lookup"><span data-stu-id="df28a-166">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="df28a-167">String</span><span class="sxs-lookup"><span data-stu-id="df28a-167">String</span></span>            | <span data-ttu-id="df28a-168">"Przechowuj"</span><span class="sxs-lookup"><span data-stu-id="df28a-168">"Make"</span></span>                |
|                   | <span data-ttu-id="df28a-169">"Marki, modelu"</span><span class="sxs-lookup"><span data-stu-id="df28a-169">"Make,Model"</span></span> |

<span data-ttu-id="df28a-170">Akceptuje wartości jeden nagłówek lub rozdzielaną przecinkami listę wartości nagłówka, które mogą powodować odświeżenie pamięci podręcznej podczas zmiany wartości parametru danych trasy.</span><span class="sxs-lookup"><span data-stu-id="df28a-170">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="df28a-171">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df28a-171">Example:</span></span>

<span data-ttu-id="df28a-172">*Startup.CS*</span><span class="sxs-lookup"><span data-stu-id="df28a-172">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="df28a-173">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="df28a-173">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="df28a-174">różnią się przez cookie</span><span class="sxs-lookup"><span data-stu-id="df28a-174">vary-by-cookie</span></span>

| <span data-ttu-id="df28a-175">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df28a-175">Attribute Type</span></span>    | <span data-ttu-id="df28a-176">Przykładowe wartości</span><span class="sxs-lookup"><span data-stu-id="df28a-176">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="df28a-177">String</span><span class="sxs-lookup"><span data-stu-id="df28a-177">String</span></span>            | <span data-ttu-id="df28a-178">". AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="df28a-178">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="df28a-179">". AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="df28a-179">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="df28a-180">Akceptuje wartości jeden nagłówek lub rozdzielaną przecinkami listę wartości nagłówka, które mogą powodować odświeżenie pamięci podręcznej podczas zmiany (s) wartości nagłówka.</span><span class="sxs-lookup"><span data-stu-id="df28a-180">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="df28a-181">Poniższy przykład analizuje plik cookie skojarzone z tożsamością ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="df28a-181">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="df28a-182">Gdy użytkownik jest uwierzytelniany żądania plik cookie należy ustawić wartość, która wyzwala odświeżania pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="df28a-182">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="df28a-183">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df28a-183">Example:</span></span>

```cshtml
<Cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="df28a-184">różnią się przez użytkownika</span><span class="sxs-lookup"><span data-stu-id="df28a-184">vary-by-user</span></span>

| <span data-ttu-id="df28a-185">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df28a-185">Attribute Type</span></span>    | <span data-ttu-id="df28a-186">Przykładowe wartości</span><span class="sxs-lookup"><span data-stu-id="df28a-186">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="df28a-187">Boolean</span><span class="sxs-lookup"><span data-stu-id="df28a-187">Boolean</span></span>             | <span data-ttu-id="df28a-188">wartość "prawda"</span><span class="sxs-lookup"><span data-stu-id="df28a-188">"true"</span></span>                  |
|                     | <span data-ttu-id="df28a-189">wartość "false" (ustawienie domyślne)</span><span class="sxs-lookup"><span data-stu-id="df28a-189">"false" (default)</span></span> |

<span data-ttu-id="df28a-190">Określa, czy pamięć podręczna powinni resetować po zmianie zalogowanego użytkownika (lub jej podmiot kontekstu zabezpieczeń).</span><span class="sxs-lookup"><span data-stu-id="df28a-190">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="df28a-191">Bieżący użytkownik jest także znana jako podmiot zabezpieczeń kontekstu żądania i mogą być wyświetlane w widoku Razor odwołując `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="df28a-191">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="df28a-192">Poniższy przykład analizuje aktualnie zalogowanego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="df28a-192">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="df28a-193">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df28a-193">Example:</span></span>

```cshtml
<Cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="df28a-194">Za pomocą tego atrybutu przechowuje zawartość w pamięci podręcznej za pomocą logowania i wyloguj się cyklu.</span><span class="sxs-lookup"><span data-stu-id="df28a-194">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="df28a-195">Korzystając z `vary-by-user="true"`, działania logowania i wyloguj się unieważnia pamięci podręcznej dla tego uwierzytelnionego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="df28a-195">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="df28a-196">Pamięć podręczna jest unieważniona, ponieważ nowa wartość unikatowy plik cookie jest generowany podczas logowania.</span><span class="sxs-lookup"><span data-stu-id="df28a-196">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="df28a-197">Pamięci podręcznej jest utrzymywana dla anonimowego stanu pliki cookie nie istnieje lub utracił ważność.</span><span class="sxs-lookup"><span data-stu-id="df28a-197">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="df28a-198">Oznacza to, gdy żaden użytkownik nie jest zalogowany, będzie przechowywany pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="df28a-198">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="df28a-199">różnią się przez</span><span class="sxs-lookup"><span data-stu-id="df28a-199">vary-by</span></span>

| <span data-ttu-id="df28a-200">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df28a-200">Attribute Type</span></span>    | <span data-ttu-id="df28a-201">Przykładowe wartości</span><span class="sxs-lookup"><span data-stu-id="df28a-201">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="df28a-202">String</span><span class="sxs-lookup"><span data-stu-id="df28a-202">String</span></span>             | <span data-ttu-id="df28a-203">"@Model"</span><span class="sxs-lookup"><span data-stu-id="df28a-203">"@Model"</span></span>                 |


<span data-ttu-id="df28a-204">Umożliwia dostosowanie pobiera buforowane dane.</span><span class="sxs-lookup"><span data-stu-id="df28a-204">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="df28a-205">Gdy obiekt odwołuje się zmian wartości atrybutu ciąg, zawartości pamięci podręcznej pomocnika tagów jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="df28a-205">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="df28a-206">Często ciągów wartości modelu są przypisane do tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="df28a-206">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="df28a-207">Efektywne oznacza to, że aktualizacja dowolną z wartości z połączonych unieważnia pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="df28a-207">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="df28a-208">W poniższym przykładzie założono metody renderowania widoku sum wartość całkowita dwóch parametrów trasy, `myParam1` i `myParam2`i zwraca go jako właściwość pojedynczego modelu.</span><span class="sxs-lookup"><span data-stu-id="df28a-208">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="df28a-209">Po zmianie tej sumy zawartości pamięci podręcznej pomocnika tagów jest renderowany i ponownie buforowany.</span><span class="sxs-lookup"><span data-stu-id="df28a-209">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="df28a-210">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df28a-210">Example:</span></span>

<span data-ttu-id="df28a-211">Akcja:</span><span class="sxs-lookup"><span data-stu-id="df28a-211">Action:</span></span>

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="df28a-212">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="df28a-212">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="df28a-213">priorytet</span><span class="sxs-lookup"><span data-stu-id="df28a-213">priority</span></span>

| <span data-ttu-id="df28a-214">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df28a-214">Attribute Type</span></span>    | <span data-ttu-id="df28a-215">Przykładowe wartości</span><span class="sxs-lookup"><span data-stu-id="df28a-215">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="df28a-216">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="df28a-216">CacheItemPriority</span></span>  | <span data-ttu-id="df28a-217">"High"</span><span class="sxs-lookup"><span data-stu-id="df28a-217">"High"</span></span>                   |
|                    | <span data-ttu-id="df28a-218">"Low"</span><span class="sxs-lookup"><span data-stu-id="df28a-218">"Low"</span></span> |
|                    | <span data-ttu-id="df28a-219">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="df28a-219">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="df28a-220">"Normal"</span><span class="sxs-lookup"><span data-stu-id="df28a-220">"Normal"</span></span> |

<span data-ttu-id="df28a-221">Zawiera wskazówki wykluczenia pamięci podręcznej do dostawcy wbudowanej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="df28a-221">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="df28a-222">Wyklucz serwer sieci web `Low` najpierw buforować wpisów, gdy jest wykorzystanie pamięci.</span><span class="sxs-lookup"><span data-stu-id="df28a-222">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="df28a-223">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df28a-223">Example:</span></span>

```cshtml
<Cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="df28a-224">`priority` Atrybutu nie gwarantuje określony poziom przechowywania w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="df28a-224">The `priority` attribute does not guarantee a specific level of cache retention.</span></span> <span data-ttu-id="df28a-225">`CacheItemPriority`jest tylko sugestię.</span><span class="sxs-lookup"><span data-stu-id="df28a-225">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="df28a-226">Ustawienie tego atrybutu na `NeverRemove` nie gwarantuje, że zawsze zachowywania pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="df28a-226">Setting this attribute to `NeverRemove` does not guarantee that the cache will always be retained.</span></span> <span data-ttu-id="df28a-227">Zobacz [dodatkowe zasoby](#additional-resources) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="df28a-227">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="df28a-228">Pomocnik Tag pamięci podręcznej jest zależna od [pamięci podręcznej usługi](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="df28a-228">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="df28a-229">Pamięci podręcznej pomocnika tagów dodaje usługę, jeśli nie został dodany.</span><span class="sxs-lookup"><span data-stu-id="df28a-229">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="df28a-230">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="df28a-230">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
