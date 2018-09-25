---
title: Pomocnik tagu w programie ASP.NET Core MVC pamięci podręcznej
author: pkellner
description: Dowiedz się, jak używać Pomocnik tagu pamięci podręcznej.
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 11754d2858d8f02c7eb9baac8feda9b50ddb3d79
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028158"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="c0d9b-103">Pomocnik tagu w programie ASP.NET Core MVC pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="c0d9b-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="c0d9b-104">Przez [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="c0d9b-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="c0d9b-105">Pomocnik tagu pamięci podręcznej umożliwia znacznie zwiększyć wydajność aplikacji platformy ASP.NET Core, buforując jego zawartości, wewnętrznego dostawcy pamięci podręcznej platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="c0d9b-106">Aparat widoku Razor ustawiana domyślnie `expires-after` do 20 minut.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="c0d9b-107">Następujący kod Razor buforuje daty/godziny:</span><span class="sxs-lookup"><span data-stu-id="c0d9b-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="c0d9b-108">Pierwsze żądanie strony, która zawiera `CacheTagHelper` spowoduje wyświetlenie bieżącej daty/godziny.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="c0d9b-109">Dodatkowe żądania pokaże wartość w pamięci podręcznej, dopóki pamięci podręcznej wygaśnie (domyślnie 20 minut) lub zostaną eksmitowane przez wykorzystanie pamięci.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="c0d9b-110">Można ustawić czas trwania pamięci podręcznej z następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="c0d9b-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="c0d9b-111">Atrybuty pomocnika tagów w pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="c0d9b-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="c0d9b-112">Włączone</span><span class="sxs-lookup"><span data-stu-id="c0d9b-112">enabled</span></span>    


| <span data-ttu-id="c0d9b-113">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="c0d9b-113">Attribute Type</span></span>    | <span data-ttu-id="c0d9b-114">Prawidłowe wartości</span><span class="sxs-lookup"><span data-stu-id="c0d9b-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="c0d9b-115">wartość logiczna</span><span class="sxs-lookup"><span data-stu-id="c0d9b-115">boolean</span></span>           | <span data-ttu-id="c0d9b-116">wartość "true" (ustawienie domyślne)</span><span class="sxs-lookup"><span data-stu-id="c0d9b-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="c0d9b-117">"false"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-117">"false"</span></span>   |


<span data-ttu-id="c0d9b-118">Określa, czy zawartość ujęta w Pomocnik tagu pamięci podręcznej są buforowane.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="c0d9b-119">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-119">The default is `true`.</span></span>  <span data-ttu-id="c0d9b-120">Jeśli ustawiono `false` to Pomocnik tagu pamięci podręcznej nie wpłyną buforowania w wyniku renderowania.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="c0d9b-121">Przykład:</span><span class="sxs-lookup"><span data-stu-id="c0d9b-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="c0d9b-122">expires-on</span><span class="sxs-lookup"><span data-stu-id="c0d9b-122">expires-on</span></span> 

| <span data-ttu-id="c0d9b-123">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="c0d9b-123">Attribute Type</span></span> |           <span data-ttu-id="c0d9b-124">Przykładowa wartość</span><span class="sxs-lookup"><span data-stu-id="c0d9b-124">Example Value</span></span>            |
|----------------|------------------------------------|
| <span data-ttu-id="c0d9b-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="c0d9b-125">DateTimeOffset</span></span> | <span data-ttu-id="c0d9b-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-126">"@new DateTime(2025,1,29,17,02,0)"</span></span> |

<span data-ttu-id="c0d9b-127">Ustawia Data bezwzględna wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="c0d9b-128">Poniższy przykład spowoduje buforowanie zawartości Pomocnik tagu pamięci podręcznej aż do 17:02:00 w dniu 29 stycznia 2025.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="c0d9b-129">Przykład:</span><span class="sxs-lookup"><span data-stu-id="c0d9b-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="c0d9b-130">expires-after</span><span class="sxs-lookup"><span data-stu-id="c0d9b-130">expires-after</span></span>

| <span data-ttu-id="c0d9b-131">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="c0d9b-131">Attribute Type</span></span> |        <span data-ttu-id="c0d9b-132">Przykładowa wartość</span><span class="sxs-lookup"><span data-stu-id="c0d9b-132">Example Value</span></span>         |
|----------------|------------------------------|
|    <span data-ttu-id="c0d9b-133">Przedział czasu</span><span class="sxs-lookup"><span data-stu-id="c0d9b-133">TimeSpan</span></span>    | <span data-ttu-id="c0d9b-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-134">"@TimeSpan.FromSeconds(120)"</span></span> |

<span data-ttu-id="c0d9b-135">Określa długość czasu od pierwszego żądania, aby buforować zawartość.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="c0d9b-136">Przykład:</span><span class="sxs-lookup"><span data-stu-id="c0d9b-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="c0d9b-137">Przedłużanie wygasa</span><span class="sxs-lookup"><span data-stu-id="c0d9b-137">expires-sliding</span></span>

| <span data-ttu-id="c0d9b-138">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="c0d9b-138">Attribute Type</span></span> |        <span data-ttu-id="c0d9b-139">Przykładowa wartość</span><span class="sxs-lookup"><span data-stu-id="c0d9b-139">Example Value</span></span>        |
|----------------|-----------------------------|
|    <span data-ttu-id="c0d9b-140">Przedział czasu</span><span class="sxs-lookup"><span data-stu-id="c0d9b-140">TimeSpan</span></span>    | <span data-ttu-id="c0d9b-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-141">"@TimeSpan.FromSeconds(60)"</span></span> |

<span data-ttu-id="c0d9b-142">Ustawia czas, który wykluczyć wpisu pamięci podręcznej, jeśli nie uzyska dostępu.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="c0d9b-143">Przykład:</span><span class="sxs-lookup"><span data-stu-id="c0d9b-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="c0d9b-144">różnią się w nagłówku</span><span class="sxs-lookup"><span data-stu-id="c0d9b-144">vary-by-header</span></span>

| <span data-ttu-id="c0d9b-145">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="c0d9b-145">Attribute Type</span></span>    | <span data-ttu-id="c0d9b-146">Przykładowe wartości</span><span class="sxs-lookup"><span data-stu-id="c0d9b-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="c0d9b-147">String</span><span class="sxs-lookup"><span data-stu-id="c0d9b-147">String</span></span>            | <span data-ttu-id="c0d9b-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="c0d9b-149">"User-Agent,content-encoding"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="c0d9b-150">Akceptuje wartość jednego nagłówka, lub rozdzielaną przecinkami listę wartości nagłówka, które mogą powodować odświeżanie pamięci podręcznej, gdy zmienią się one.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="c0d9b-151">Poniższy przykład monitoruje wartość nagłówka `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="c0d9b-152">Przykład będzie buforować zawartość dla każdego innego `User-Agent` przesłanym do serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="c0d9b-153">Przykład:</span><span class="sxs-lookup"><span data-stu-id="c0d9b-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="c0d9b-154">różnią się przez zapytanie</span><span class="sxs-lookup"><span data-stu-id="c0d9b-154">vary-by-query</span></span>

| <span data-ttu-id="c0d9b-155">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="c0d9b-155">Attribute Type</span></span>    | <span data-ttu-id="c0d9b-156">Przykładowe wartości</span><span class="sxs-lookup"><span data-stu-id="c0d9b-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="c0d9b-157">String</span><span class="sxs-lookup"><span data-stu-id="c0d9b-157">String</span></span>            | <span data-ttu-id="c0d9b-158">"Przechowuj"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-158">"Make"</span></span>                |
|                   | <span data-ttu-id="c0d9b-159">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-159">"Make,Model"</span></span> |

<span data-ttu-id="c0d9b-160">Akceptuje wartość jednego nagłówka, lub rozdzielaną przecinkami listę wartości nagłówka, które wyzwalać odświeżanie pamięci podręcznej po zmianie wartości nagłówka.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="c0d9b-161">Poniższy przykład sprawdza wartości `Make` i `Model`.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="c0d9b-162">Przykład:</span><span class="sxs-lookup"><span data-stu-id="c0d9b-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="c0d9b-163">różnią się przez trasy</span><span class="sxs-lookup"><span data-stu-id="c0d9b-163">vary-by-route</span></span>

| <span data-ttu-id="c0d9b-164">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="c0d9b-164">Attribute Type</span></span>    | <span data-ttu-id="c0d9b-165">Przykładowe wartości</span><span class="sxs-lookup"><span data-stu-id="c0d9b-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="c0d9b-166">String</span><span class="sxs-lookup"><span data-stu-id="c0d9b-166">String</span></span>            | <span data-ttu-id="c0d9b-167">"Przechowuj"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-167">"Make"</span></span>                |
|                   | <span data-ttu-id="c0d9b-168">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-168">"Make,Model"</span></span> |

<span data-ttu-id="c0d9b-169">Akceptuje wartość jednego nagłówka, lub rozdzielaną przecinkami listę wartości nagłówka, które mogą powodować odświeżanie pamięci podręcznej, gdy parametr danych trasy wartości zmiany.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="c0d9b-170">Przykład:</span><span class="sxs-lookup"><span data-stu-id="c0d9b-170">Example:</span></span>

<span data-ttu-id="c0d9b-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="c0d9b-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="c0d9b-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c0d9b-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="c0d9b-173">różnią się przez cookie</span><span class="sxs-lookup"><span data-stu-id="c0d9b-173">vary-by-cookie</span></span>

| <span data-ttu-id="c0d9b-174">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="c0d9b-174">Attribute Type</span></span>    | <span data-ttu-id="c0d9b-175">Przykładowe wartości</span><span class="sxs-lookup"><span data-stu-id="c0d9b-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="c0d9b-176">String</span><span class="sxs-lookup"><span data-stu-id="c0d9b-176">String</span></span>            | <span data-ttu-id="c0d9b-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="c0d9b-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="c0d9b-179">Akceptuje wartość jednego nagłówka, lub rozdzielaną przecinkami listę wartości nagłówka, które mogą powodować odświeżanie pamięci podręcznej, gdy zmiany (s) wartości nagłówka.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="c0d9b-180">Poniższy przykład sprawdza plik cookie skojarzone z tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-180">The following example looks at the cookie associated with ASP.NET Core Identity.</span></span> <span data-ttu-id="c0d9b-181">Gdy użytkownik jest uwierzytelniany żądania plik cookie należy ustawić, co powoduje wyzwolenie odświeżania pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="c0d9b-182">Przykład:</span><span class="sxs-lookup"><span data-stu-id="c0d9b-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="c0d9b-183">różnią się przez użytkownika</span><span class="sxs-lookup"><span data-stu-id="c0d9b-183">vary-by-user</span></span>

| <span data-ttu-id="c0d9b-184">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="c0d9b-184">Attribute Type</span></span>    | <span data-ttu-id="c0d9b-185">Przykładowe wartości</span><span class="sxs-lookup"><span data-stu-id="c0d9b-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="c0d9b-186">Boolean</span><span class="sxs-lookup"><span data-stu-id="c0d9b-186">Boolean</span></span>             | <span data-ttu-id="c0d9b-187">wartość "prawda"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-187">"true"</span></span>                  |
|                     | <span data-ttu-id="c0d9b-188">"false" (ustawienie domyślne)</span><span class="sxs-lookup"><span data-stu-id="c0d9b-188">"false" (default)</span></span> |

<span data-ttu-id="c0d9b-189">Określa, czy pamięć podręczną należy resetować po zmianie zalogowanego użytkownika (lub w kontekście jednostki).</span><span class="sxs-lookup"><span data-stu-id="c0d9b-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="c0d9b-190">Bieżący użytkownik jest także znana jako jednostki kontekstu żądania i mogą być wyświetlane w widoku Razor, odwołując się do `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="c0d9b-191">Poniższy przykład sprawdza aktualnie zalogowanego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="c0d9b-192">Przykład:</span><span class="sxs-lookup"><span data-stu-id="c0d9b-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="c0d9b-193">Za pomocą tego atrybutu przechowuje zawartość w pamięci podręcznej przez cykl logowania i Wyloguj.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="c0d9b-194">Korzystając z `vary-by-user="true"`, działania logowania i wyloguj unieważnia zawartość pamięci podręcznej dla tego uwierzytelnionego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="c0d9b-195">Pamięć podręczna jest unieważnione, ponieważ nowa wartość unikatowego pliku cookie jest generowany podczas logowania.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="c0d9b-196">Pamięć podręczna jest zachowywana na potrzeby stanu anonimowe pliki cookie nie istnieje lub utracił ważność.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="c0d9b-197">Oznacza to, jeśli żaden użytkownik nie jest zalogowany, pamięci podręcznej zostaną zachowane.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="c0d9b-198">różnią się przez</span><span class="sxs-lookup"><span data-stu-id="c0d9b-198">vary-by</span></span>

| <span data-ttu-id="c0d9b-199">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="c0d9b-199">Attribute Type</span></span> | <span data-ttu-id="c0d9b-200">Przykładowe wartości</span><span class="sxs-lookup"><span data-stu-id="c0d9b-200">Example Values</span></span> |
|----------------|----------------|
|     <span data-ttu-id="c0d9b-201">String</span><span class="sxs-lookup"><span data-stu-id="c0d9b-201">String</span></span>     |    <span data-ttu-id="c0d9b-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-202">"@Model"</span></span>    |

<span data-ttu-id="c0d9b-203">Umożliwia dostosowanie pobiera buforowane dane.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="c0d9b-204">Gdy zostanie zaktualizowany obiekt odwołuje się ten atrybut ciągu wartości zmiany, zawartość Pomocnik tagu pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="c0d9b-205">Często ciągów wartości modelu są przypisane do tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="c0d9b-206">Skutecznie oznacza to, że aktualizacja dowolną z wartości z połączonych unieważnia zawartość pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="c0d9b-207">W poniższym przykładzie założono metody kontrolera renderowania sum widoku wartość całkowitą dwa parametry trasy `myParam1` i `myParam2`i zwraca ją jako właściwość pojedynczego modelu.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="c0d9b-208">Po zmianie tej sumy zawartość Pomocnik tagu pamięci podręcznej jest renderowana i ponownie buforowany.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="c0d9b-209">Przykład:</span><span class="sxs-lookup"><span data-stu-id="c0d9b-209">Example:</span></span>

<span data-ttu-id="c0d9b-210">Akcja:</span><span class="sxs-lookup"><span data-stu-id="c0d9b-210">Action:</span></span>

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

<span data-ttu-id="c0d9b-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c0d9b-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="c0d9b-212">priorytet</span><span class="sxs-lookup"><span data-stu-id="c0d9b-212">priority</span></span>

| <span data-ttu-id="c0d9b-213">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="c0d9b-213">Attribute Type</span></span>    | <span data-ttu-id="c0d9b-214">Przykładowe wartości</span><span class="sxs-lookup"><span data-stu-id="c0d9b-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="c0d9b-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="c0d9b-215">CacheItemPriority</span></span>  | <span data-ttu-id="c0d9b-216">"Wysoka"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-216">"High"</span></span>                   |
|                    | <span data-ttu-id="c0d9b-217">"Niska"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-217">"Low"</span></span> |
|                    | <span data-ttu-id="c0d9b-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="c0d9b-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="c0d9b-219">"Normal"</span></span> |

<span data-ttu-id="c0d9b-220">Znajdują się wskazówki eksmisji pamięci podręcznej dostawcy wbudowaną pamięć podręczną.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="c0d9b-221">Wyklucz serwer sieci web `Low` najpierw pamięci podręcznej wpisów, po duże wykorzystanie pamięci.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="c0d9b-222">Przykład:</span><span class="sxs-lookup"><span data-stu-id="c0d9b-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="c0d9b-223">`priority` Atrybutu nie gwarantuje określony poziom przechowywania w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="c0d9b-224">`CacheItemPriority` jest tylko sugestię.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="c0d9b-225">Ustawienie tego atrybutu na `NeverRemove` nie gwarantuje pamięci podręcznej zawsze zostaną zachowane.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="c0d9b-226">Zobacz [dodatkowe zasoby](#additional-resources) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="c0d9b-227">Pomocnik tagu pamięci podręcznej jest zależny od [usługa pamięci podręcznej pamięci](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="c0d9b-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="c0d9b-228">Pomocnik tagu pamięci podręcznej dodaje usługę, jeśli nie został dodany.</span><span class="sxs-lookup"><span data-stu-id="c0d9b-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c0d9b-229">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c0d9b-229">Additional resources</span></span>

* [<span data-ttu-id="c0d9b-230">Buforowanie w pamięci</span><span class="sxs-lookup"><span data-stu-id="c0d9b-230">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="c0d9b-231">Wprowadzenie do tożsamości</span><span class="sxs-lookup"><span data-stu-id="c0d9b-231">Introduction to Identity</span></span>](xref:security/authentication/identity)
