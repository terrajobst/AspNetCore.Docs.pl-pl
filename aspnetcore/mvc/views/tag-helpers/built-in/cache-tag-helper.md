---
title: Pomocnik tagu w programie ASP.NET Core MVC pamięci podręcznej
author: pkellner
description: Dowiedz się, jak używać Pomocnik tagu pamięci podręcznej.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: fb69584f6e9d4756e175bbd6f3deb1f413b80fc5
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244817"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="df411-103">Pomocnik tagu w programie ASP.NET Core MVC pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="df411-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="df411-104">Przez [Peter Kellner](http://peterkellner.net) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="df411-104">By [Peter Kellner](http://peterkellner.net) and [Luke Latham](https://github.com/guardrex)</span></span> 

<span data-ttu-id="df411-105">Pomocnik tagu pamięci podręcznej umożliwia poprawę wydajności aplikacji platformy ASP.NET Core przez buforowanie jego zawartości, wewnętrznego dostawcy pamięci podręcznej platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="df411-105">The Cache Tag Helper provides the ability to improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="df411-106">Aby zapoznać się z omówieniem pomocnicy tagów, zobacz <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="df411-106">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="df411-107">Następujący kod Razor buforuje bieżącą datą:</span><span class="sxs-lookup"><span data-stu-id="df411-107">The following Razor markup caches the current date:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="df411-108">Pierwsze żądanie strony, która zawiera Pomocnik tagu Wyświetla bieżącą datę.</span><span class="sxs-lookup"><span data-stu-id="df411-108">The first request to the page that contains the Tag Helper displays the current date.</span></span> <span data-ttu-id="df411-109">Wartość w pamięci podręcznej umożliwia wyświetlenie dodatkowych żądań do momentu wygaśnięcia pamięci podręcznej (domyślnie 20 minut) lub daty pamięci podręcznej zostanie usunięty z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="df411-109">Additional requests show the cached value until the cache expires (default 20 minutes) or until the cached date is evicted from the cache.</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="df411-110">Atrybuty pomocnika tagów w pamięci podręcznej</span><span class="sxs-lookup"><span data-stu-id="df411-110">Cache Tag Helper Attributes</span></span>

### <a name="enabled"></a><span data-ttu-id="df411-111">Włączone</span><span class="sxs-lookup"><span data-stu-id="df411-111">enabled</span></span>

| <span data-ttu-id="df411-112">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df411-112">Attribute Type</span></span>  | <span data-ttu-id="df411-113">Przykłady</span><span class="sxs-lookup"><span data-stu-id="df411-113">Examples</span></span>        | <span data-ttu-id="df411-114">Domyślny</span><span class="sxs-lookup"><span data-stu-id="df411-114">Default</span></span> |
| --------------- | --------------- | ------- |
| <span data-ttu-id="df411-115">Boolean</span><span class="sxs-lookup"><span data-stu-id="df411-115">Boolean</span></span>         | <span data-ttu-id="df411-116">`true`, `false`</span><span class="sxs-lookup"><span data-stu-id="df411-116">`true`, `false`</span></span> | `true`  |

<span data-ttu-id="df411-117">`enabled` Określa, jeśli zawartość ujęta w Pomocnik tagu pamięci podręcznej są buforowane.</span><span class="sxs-lookup"><span data-stu-id="df411-117">`enabled` determines if the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="df411-118">Wartość domyślna to `true`.</span><span class="sxs-lookup"><span data-stu-id="df411-118">The default is `true`.</span></span> <span data-ttu-id="df411-119">Jeśli ustawiono `false`, jest wyniku renderowania **nie** pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="df411-119">If set to `false`, the rendered output is **not** cached.</span></span>

<span data-ttu-id="df411-120">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df411-120">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-on"></a><span data-ttu-id="df411-121">expires-on</span><span class="sxs-lookup"><span data-stu-id="df411-121">expires-on</span></span>

| <span data-ttu-id="df411-122">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df411-122">Attribute Type</span></span>   | <span data-ttu-id="df411-123">Przykład</span><span class="sxs-lookup"><span data-stu-id="df411-123">Example</span></span>                            |
| ---------------- | ---------------------------------- |
| `DateTimeOffset` | `@new DateTime(2025,1,29,17,02,0)` |

<span data-ttu-id="df411-124">`expires-on` Ustawia bezwzględnych wygaśnięcia elementu pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="df411-124">`expires-on` sets an absolute expiration date for the cached item.</span></span>

<span data-ttu-id="df411-125">Poniższy przykład zapisuje w pamięci podręcznej zawartość Pomocnik tagu pamięci podręcznej aż do 17:02:00 w dniu 29 stycznia 2025:</span><span class="sxs-lookup"><span data-stu-id="df411-125">The following example caches the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-after"></a><span data-ttu-id="df411-126">expires-after</span><span class="sxs-lookup"><span data-stu-id="df411-126">expires-after</span></span>

| <span data-ttu-id="df411-127">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df411-127">Attribute Type</span></span> | <span data-ttu-id="df411-128">Przykład</span><span class="sxs-lookup"><span data-stu-id="df411-128">Example</span></span>                      | <span data-ttu-id="df411-129">Domyślny</span><span class="sxs-lookup"><span data-stu-id="df411-129">Default</span></span>    |
| -------------- | ---------------------------- | ---------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(120)` | <span data-ttu-id="df411-130">20 minut</span><span class="sxs-lookup"><span data-stu-id="df411-130">20 minutes</span></span> |

<span data-ttu-id="df411-131">`expires-after` Określa długość czasu od pierwszego żądania, aby buforować zawartość.</span><span class="sxs-lookup"><span data-stu-id="df411-131">`expires-after` sets the length of time from the first request time to cache the contents.</span></span>

<span data-ttu-id="df411-132">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df411-132">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="df411-133">Aparat widoku Razor ustawiana domyślnie `expires-after` wartość do 20 minut.</span><span class="sxs-lookup"><span data-stu-id="df411-133">The Razor View Engine sets the default `expires-after` value to twenty minutes.</span></span>

### <a name="expires-sliding"></a><span data-ttu-id="df411-134">Przedłużanie wygasa</span><span class="sxs-lookup"><span data-stu-id="df411-134">expires-sliding</span></span>

| <span data-ttu-id="df411-135">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df411-135">Attribute Type</span></span> | <span data-ttu-id="df411-136">Przykład</span><span class="sxs-lookup"><span data-stu-id="df411-136">Example</span></span>                     |
| -------------- | --------------------------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(60)` |

<span data-ttu-id="df411-137">Ustawia czas, który powinien zostać wykluczony wpisu pamięci podręcznej, jeśli jego wartość nie uzyskano.</span><span class="sxs-lookup"><span data-stu-id="df411-137">Sets the time that a cache entry should be evicted if its value hasn't been accessed.</span></span>

<span data-ttu-id="df411-138">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df411-138">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-header"></a><span data-ttu-id="df411-139">różnią się w nagłówku</span><span class="sxs-lookup"><span data-stu-id="df411-139">vary-by-header</span></span>

| <span data-ttu-id="df411-140">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df411-140">Attribute Type</span></span> | <span data-ttu-id="df411-141">Przykłady</span><span class="sxs-lookup"><span data-stu-id="df411-141">Examples</span></span>                                    |
| -------------- | ------------------------------------------- |
| <span data-ttu-id="df411-142">String</span><span class="sxs-lookup"><span data-stu-id="df411-142">String</span></span>         | <span data-ttu-id="df411-143">`User-Agent`, `User-Agent,content-encoding`</span><span class="sxs-lookup"><span data-stu-id="df411-143">`User-Agent`, `User-Agent,content-encoding`</span></span> |

<span data-ttu-id="df411-144">`vary-by-header` akceptuje rozdzielana przecinkami lista wartości nagłówka, które wyzwalać odświeżanie pamięci podręcznej po zmianie.</span><span class="sxs-lookup"><span data-stu-id="df411-144">`vary-by-header` accepts a comma-delimited list of header values that trigger a cache refresh when they change.</span></span>

<span data-ttu-id="df411-145">Poniższy przykład monitoruje wartość nagłówka `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="df411-145">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="df411-146">Przykład buforuje zawartość dla każdego innego `User-Agent` przesłanym do serwera sieci web:</span><span class="sxs-lookup"><span data-stu-id="df411-146">The example caches the content for every different `User-Agent` presented to the web server:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-query"></a><span data-ttu-id="df411-147">różnią się przez zapytanie</span><span class="sxs-lookup"><span data-stu-id="df411-147">vary-by-query</span></span>

| <span data-ttu-id="df411-148">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df411-148">Attribute Type</span></span> | <span data-ttu-id="df411-149">Przykłady</span><span class="sxs-lookup"><span data-stu-id="df411-149">Examples</span></span>             |
| -------------- | -------------------- |
| <span data-ttu-id="df411-150">String</span><span class="sxs-lookup"><span data-stu-id="df411-150">String</span></span>         | <span data-ttu-id="df411-151">`Make`, `Make,Model`</span><span class="sxs-lookup"><span data-stu-id="df411-151">`Make`, `Make,Model`</span></span> |

<span data-ttu-id="df411-152">`vary-by-query` akceptuje rozdzielaną przecinkami listę <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> w ciągu zapytania (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>) który wyzwolić odświeżanie pamięci podręcznej w przypadku wartości dowolnych zmian klucza na liście.</span><span class="sxs-lookup"><span data-stu-id="df411-152">`vary-by-query` accepts a comma-delimited list of <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> in a query string (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>) that trigger a cache refresh when the value of any listed key changes.</span></span>

<span data-ttu-id="df411-153">Poniższy przykład monitoruje wartości `Make` i `Model`.</span><span class="sxs-lookup"><span data-stu-id="df411-153">The following example monitors the values of `Make` and `Model`.</span></span> <span data-ttu-id="df411-154">Przykład buforuje zawartość dla każdego innego `Make` i `Model` przesłanym do serwera sieci web:</span><span class="sxs-lookup"><span data-stu-id="df411-154">The example caches the content for every different `Make` and `Model` presented to the web server:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-route"></a><span data-ttu-id="df411-155">różnią się przez trasy</span><span class="sxs-lookup"><span data-stu-id="df411-155">vary-by-route</span></span>

| <span data-ttu-id="df411-156">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df411-156">Attribute Type</span></span> | <span data-ttu-id="df411-157">Przykłady</span><span class="sxs-lookup"><span data-stu-id="df411-157">Examples</span></span>             |
| -------------- | -------------------- |
| <span data-ttu-id="df411-158">String</span><span class="sxs-lookup"><span data-stu-id="df411-158">String</span></span>         | <span data-ttu-id="df411-159">`Make`, `Make,Model`</span><span class="sxs-lookup"><span data-stu-id="df411-159">`Make`, `Make,Model`</span></span> |

<span data-ttu-id="df411-160">`vary-by-route` akceptuje rozdzielaną przecinkami listę nazw parametrów trasy, które mogą powodować odświeżanie pamięci podręcznej po zmianie wartości parametru danych trasy.</span><span class="sxs-lookup"><span data-stu-id="df411-160">`vary-by-route` accepts a comma-delimited list of route parameter names that trigger a cache refresh when the route data parameter value changes.</span></span>

<span data-ttu-id="df411-161">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df411-161">Example:</span></span>

<span data-ttu-id="df411-162">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="df411-162">*Startup.cs*:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="df411-163">*Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="df411-163">*Index.cshtml*:</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-cookie"></a><span data-ttu-id="df411-164">różnią się przez cookie</span><span class="sxs-lookup"><span data-stu-id="df411-164">vary-by-cookie</span></span>

| <span data-ttu-id="df411-165">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df411-165">Attribute Type</span></span> | <span data-ttu-id="df411-166">Przykłady</span><span class="sxs-lookup"><span data-stu-id="df411-166">Examples</span></span>                                                                         |
| -------------- | -------------------------------------------------------------------------------- |
| <span data-ttu-id="df411-167">String</span><span class="sxs-lookup"><span data-stu-id="df411-167">String</span></span>         | <span data-ttu-id="df411-168">`.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor`</span><span class="sxs-lookup"><span data-stu-id="df411-168">`.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor`</span></span> |

<span data-ttu-id="df411-169">`vary-by-cookie` akceptuje rozdzielana przecinkami lista nazw plików cookie, które wyzwalać odświeżanie pamięci podręcznej po zmianie wartości pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="df411-169">`vary-by-cookie` accepts a comma-delimited list of cookie names that trigger a cache refresh when the cookie values change.</span></span>

<span data-ttu-id="df411-170">Poniższy przykład monitoruje pliki cookie skojarzone z tożsamości platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="df411-170">The following example monitors the cookie associated with ASP.NET Core Identity.</span></span> <span data-ttu-id="df411-171">Gdy użytkownik jest uwierzytelniany, zmiany w pliku cookie tożsamości wyzwala odświeżanie pamięci podręcznej:</span><span class="sxs-lookup"><span data-stu-id="df411-171">When a user is authenticated, a change in the Identity cookie triggers a cache refresh:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-user"></a><span data-ttu-id="df411-172">różnią się przez użytkownika</span><span class="sxs-lookup"><span data-stu-id="df411-172">vary-by-user</span></span>

| <span data-ttu-id="df411-173">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df411-173">Attribute Type</span></span>  | <span data-ttu-id="df411-174">Przykłady</span><span class="sxs-lookup"><span data-stu-id="df411-174">Examples</span></span>        | <span data-ttu-id="df411-175">Domyślny</span><span class="sxs-lookup"><span data-stu-id="df411-175">Default</span></span> |
| --------------- | --------------- | ------- |
| <span data-ttu-id="df411-176">Boolean</span><span class="sxs-lookup"><span data-stu-id="df411-176">Boolean</span></span>         | <span data-ttu-id="df411-177">`true`, `false`</span><span class="sxs-lookup"><span data-stu-id="df411-177">`true`, `false`</span></span> | `true`  |

<span data-ttu-id="df411-178">`vary-by-user` Określa, czy pamięć podręczna resetuje po zmianie zalogowanego użytkownika (lub w kontekście jednostki).</span><span class="sxs-lookup"><span data-stu-id="df411-178">`vary-by-user` specifies whether or not the cache resets when the signed-in user (or Context Principal) changes.</span></span> <span data-ttu-id="df411-179">Bieżący użytkownik jest także znana jako jednostki kontekstu żądania i mogą być wyświetlane w widoku Razor, odwołując się do `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="df411-179">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="df411-180">Poniższy przykład monitoruje bieżącego zalogowanego użytkownika, aby wyzwolić odświeżanie pamięci podręcznej:</span><span class="sxs-lookup"><span data-stu-id="df411-180">The following example monitors the current logged in user to trigger a cache refresh:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="df411-181">Za pomocą tego atrybutu przechowuje zawartość w pamięci podręcznej, za pośrednictwem logowania i wylogowania cyklu.</span><span class="sxs-lookup"><span data-stu-id="df411-181">Using this attribute maintains the contents in cache through a sign-in and sign-out cycle.</span></span> <span data-ttu-id="df411-182">Jeśli wartość jest równa `true`, cyklu uwierzytelniania unieważnia zawartość pamięci podręcznej dla tego uwierzytelnionego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="df411-182">When the value is set to `true`, an authentication cycle invalidates the cache for the authenticated user.</span></span> <span data-ttu-id="df411-183">Pamięć podręczna jest unieważnione, ponieważ nowa wartość unikatowego pliku cookie jest generowany, gdy użytkownik jest uwierzytelniany.</span><span class="sxs-lookup"><span data-stu-id="df411-183">The cache is invalidated because a new unique cookie value is generated when a user is authenticated.</span></span> <span data-ttu-id="df411-184">Pamięć podręczna jest zachowywana na potrzeby stanu anonimowe pliki cookie nie są obecne lub plik cookie utracił ważność.</span><span class="sxs-lookup"><span data-stu-id="df411-184">Cache is maintained for the anonymous state when no cookie is present or the cookie has expired.</span></span> <span data-ttu-id="df411-185">Jeśli użytkownik jest **nie** uwierzytelniony, pamięci podręcznej jest utrzymywana.</span><span class="sxs-lookup"><span data-stu-id="df411-185">If the user is **not** authenticated, the cache is maintained.</span></span>

### <a name="vary-by"></a><span data-ttu-id="df411-186">różnią się przez</span><span class="sxs-lookup"><span data-stu-id="df411-186">vary-by</span></span>

| <span data-ttu-id="df411-187">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df411-187">Attribute Type</span></span> | <span data-ttu-id="df411-188">Przykład</span><span class="sxs-lookup"><span data-stu-id="df411-188">Example</span></span>  |
| -------------- | -------- |
| <span data-ttu-id="df411-189">String</span><span class="sxs-lookup"><span data-stu-id="df411-189">String</span></span>         | `@Model` |

<span data-ttu-id="df411-190">`vary-by` Umożliwia dostosowanie jakie dane są buforowane.</span><span class="sxs-lookup"><span data-stu-id="df411-190">`vary-by` allows for customization of what data is cached.</span></span> <span data-ttu-id="df411-191">Gdy zostanie zaktualizowany obiekt odwołuje się ten atrybut ciągu wartości zmiany, zawartość Pomocnik tagu pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="df411-191">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="df411-192">Często ciągów wartości modelu są przypisane do tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="df411-192">Often, a string-concatenation of model values are assigned to this attribute.</span></span> <span data-ttu-id="df411-193">Skutecznie powoduje to scenariusz, w którym aktualizacji do dowolnej wartości łączonych unieważnia zawartość pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="df411-193">Effectively, this results in a scenario where an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="df411-194">W poniższym przykładzie założono metody kontrolera renderowania sum widoku wartość całkowitą dwa parametry trasy `myParam1` i `myParam2`i zwraca sumę jako właściwość pojedynczego modelu.</span><span class="sxs-lookup"><span data-stu-id="df411-194">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns the sum as the single model property.</span></span> <span data-ttu-id="df411-195">Po zmianie tej sumy zawartość Pomocnik tagu pamięci podręcznej jest renderowana i ponownie buforowany.</span><span class="sxs-lookup"><span data-stu-id="df411-195">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="df411-196">Akcja:</span><span class="sxs-lookup"><span data-stu-id="df411-196">Action:</span></span>

```csharp
public IActionResult Index(string myParam1, string myParam2, string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="df411-197">*Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="df411-197">*Index.cshtml*:</span></span>

```cshtml
<cache vary-by="@Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="priority"></a><span data-ttu-id="df411-198">priorytet</span><span class="sxs-lookup"><span data-stu-id="df411-198">priority</span></span>

| <span data-ttu-id="df411-199">Typ atrybutu</span><span class="sxs-lookup"><span data-stu-id="df411-199">Attribute Type</span></span>      | <span data-ttu-id="df411-200">Przykłady</span><span class="sxs-lookup"><span data-stu-id="df411-200">Examples</span></span>                               | <span data-ttu-id="df411-201">Domyślny</span><span class="sxs-lookup"><span data-stu-id="df411-201">Default</span></span>  |
| ------------------- | -------------------------------------- | -------- |
| `CacheItemPriority` | <span data-ttu-id="df411-202">`High`, `Low`, `NeverRemove`, `Normal`</span><span class="sxs-lookup"><span data-stu-id="df411-202">`High`, `Low`, `NeverRemove`, `Normal`</span></span> | `Normal` |

<span data-ttu-id="df411-203">`priority` Znajdują się wskazówki eksmisji pamięci podręcznej dostawcy wbudowaną pamięć podręczną.</span><span class="sxs-lookup"><span data-stu-id="df411-203">`priority` provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="df411-204">Wyklucza serwer sieci web mogą `Low` najpierw pamięci podręcznej wpisów, po duże wykorzystanie pamięci.</span><span class="sxs-lookup"><span data-stu-id="df411-204">The web server evicts `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="df411-205">Przykład:</span><span class="sxs-lookup"><span data-stu-id="df411-205">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="df411-206">`priority` Atrybutu nie gwarantuje określony poziom przechowywania w pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="df411-206">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="df411-207">`CacheItemPriority` jest tylko sugestię.</span><span class="sxs-lookup"><span data-stu-id="df411-207">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="df411-208">Ustawienie tego atrybutu na `NeverRemove` nie gwarantuje, że elementy pamięci podręcznej są zawsze zachowywane.</span><span class="sxs-lookup"><span data-stu-id="df411-208">Setting this attribute to `NeverRemove` doesn't guarantee that cached items are always retained.</span></span> <span data-ttu-id="df411-209">Zobacz Tematy w [dodatkowe zasoby](#additional-resources) sekcji, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="df411-209">See the topics in the [Additional Resources](#additional-resources) section for more information.</span></span>

<span data-ttu-id="df411-210">Pomocnik tagu pamięci podręcznej jest zależny od [usługa pamięci podręcznej pamięci](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="df411-210">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="df411-211">Pomocnik tagu pamięci podręcznej dodaje usługę, jeśli nie została dodana.</span><span class="sxs-lookup"><span data-stu-id="df411-211">The Cache Tag Helper adds the service if it hasn't been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="df411-212">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="df411-212">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
