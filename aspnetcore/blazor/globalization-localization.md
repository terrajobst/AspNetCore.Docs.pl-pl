---
title: ASP.NET Core Blazor globalizacja i lokalizacja
author: guardrex
description: Dowiedz się, jak udostępnić składniki Razor użytkownikom w wielu kulturach i językach.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/globalization-localization
ms.openlocfilehash: aba62fa7b6285c8ba884652694f1ea3e3a66ed18
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655492"
---
# <a name="aspnet-core-opno-locblazor-globalization-and-localization"></a><span data-ttu-id="5bd85-103">ASP.NET Core Blazor globalizacja i lokalizacja</span><span class="sxs-lookup"><span data-stu-id="5bd85-103">ASP.NET Core Blazor globalization and localization</span></span>

<span data-ttu-id="5bd85-104">Autorzy [Luke Latham](https://github.com/guardrex) i [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="5bd85-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="5bd85-105">Składniki Razor można udostępnić użytkownikom w wielu kulturach i językach.</span><span class="sxs-lookup"><span data-stu-id="5bd85-105">Razor components can be made accessible to users in multiple cultures and languages.</span></span> <span data-ttu-id="5bd85-106">Dostępne są następujące scenariusze globalizacji i lokalizacji platformy .NET:</span><span class="sxs-lookup"><span data-stu-id="5bd85-106">The following .NET globalization and localization scenarios are available:</span></span>

* <span data-ttu-id="5bd85-107">. System zasobów netto</span><span class="sxs-lookup"><span data-stu-id="5bd85-107">.NET's resources system</span></span>
* <span data-ttu-id="5bd85-108">Formatowanie liczb i dat specyficznych dla kultury</span><span class="sxs-lookup"><span data-stu-id="5bd85-108">Culture-specific number and date formatting</span></span>

<span data-ttu-id="5bd85-109">Obecnie obsługiwane są ograniczone zestawy ASP.NET Core scenariuszy lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="5bd85-109">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="5bd85-110">`IStringLocalizer<>` *jest obsługiwana* w aplikacjach Blazor.</span><span class="sxs-lookup"><span data-stu-id="5bd85-110">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="5bd85-111">Lokalizacja `IHtmlLocalizer<>`, `IViewLocalizer<>`i adnotacji danych są ASP.NET Core scenariusze MVC i **nie są obsługiwane** w aplikacjach Blazor.</span><span class="sxs-lookup"><span data-stu-id="5bd85-111">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="5bd85-112">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="5bd85-112">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="globalization"></a><span data-ttu-id="5bd85-113">Globalizacja</span><span class="sxs-lookup"><span data-stu-id="5bd85-113">Globalization</span></span>

<span data-ttu-id="5bd85-114">Funkcja `@bind` Blazorwykonuje formaty i analizuje wartości na potrzeby wyświetlania w oparciu o bieżącą kulturę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5bd85-114">Blazor's `@bind` functionality performs formats and parses values for display based on the user's current culture.</span></span>

<span data-ttu-id="5bd85-115">Dostęp do bieżącej kultury można uzyskać ze właściwości <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="5bd85-115">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="5bd85-116">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) jest używany dla następujących typów pól (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="5bd85-116">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="5bd85-117">Poprzednie typy pól:</span><span class="sxs-lookup"><span data-stu-id="5bd85-117">The preceding field types:</span></span>

* <span data-ttu-id="5bd85-118">Są wyświetlane przy użyciu odpowiednich reguł formatowania opartych na przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="5bd85-118">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="5bd85-119">Nie może zawierać tekstu o dowolnym formacie.</span><span class="sxs-lookup"><span data-stu-id="5bd85-119">Can't contain free-form text.</span></span>
* <span data-ttu-id="5bd85-120">Podaj charakterystykę interakcji użytkownika w oparciu o implementację przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="5bd85-120">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="5bd85-121">Następujące typy pól mają określone wymagania dotyczące formatowania i nie są obecnie obsługiwane przez Blazor, ponieważ nie są obsługiwane przez wszystkie główne przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="5bd85-121">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="5bd85-122">`@bind` obsługuje parametr `@bind:culture`, aby zapewnić <xref:System.Globalization.CultureInfo?displayProperty=fullName> do analizy i formatowania wartości.</span><span class="sxs-lookup"><span data-stu-id="5bd85-122">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="5bd85-123">Określanie kultury nie jest zalecane w przypadku używania typów pól `date` i `number`.</span><span class="sxs-lookup"><span data-stu-id="5bd85-123">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="5bd85-124">`date` i `number` mają wbudowaną obsługę Blazor, która udostępnia wymaganą kulturę.</span><span class="sxs-lookup"><span data-stu-id="5bd85-124">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

## <a name="localization"></a><span data-ttu-id="5bd85-125">Lokalizacja</span><span class="sxs-lookup"><span data-stu-id="5bd85-125">Localization</span></span>

<span data-ttu-id="5bd85-126">aplikacje serwera Blazor są zlokalizowane przy użyciu [oprogramowania pośredniczącego](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="5bd85-126">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="5bd85-127">Oprogramowanie pośredniczące wybiera odpowiednią kulturę dla użytkowników żądających zasobów z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5bd85-127">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="5bd85-128">Kulturę można ustawić przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="5bd85-128">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="5bd85-129">Plik cookie</span><span class="sxs-lookup"><span data-stu-id="5bd85-129">Cookies</span></span>](#cookies)
* [<span data-ttu-id="5bd85-130">Podaj interfejs użytkownika, aby wybrać kulturę</span><span class="sxs-lookup"><span data-stu-id="5bd85-130">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="5bd85-131">Aby uzyskać więcej informacji i przykładów, zobacz <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="5bd85-131">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a><span data-ttu-id="5bd85-132">Konfigurowanie konsolidatora do użytku wieloplikowego (Blazor webassembly)</span><span class="sxs-lookup"><span data-stu-id="5bd85-132">Configure the linker for internationalization (Blazor WebAssembly)</span></span>

<span data-ttu-id="5bd85-133">Domyślnie konfiguracja konsolidatora Blazordla Blazor aplikacji webassembly umożliwia rozłączenie informacji o danych wielojęzycznych z wyjątkiem lokalizacji lokalnych jawnie żądanych.</span><span class="sxs-lookup"><span data-stu-id="5bd85-133">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="5bd85-134">Aby uzyskać więcej informacji i wskazówek dotyczących kontrolowania zachowania konsolidatora, zobacz <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span><span class="sxs-lookup"><span data-stu-id="5bd85-134">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="5bd85-135">Cookie</span><span class="sxs-lookup"><span data-stu-id="5bd85-135">Cookies</span></span>

<span data-ttu-id="5bd85-136">Plik cookie kultury lokalizacji może utrzymywać kulturę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5bd85-136">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="5bd85-137">Plik cookie jest tworzony przez metodę `OnGet` strony hosta aplikacji (*strony/hosta. cshtml. cs*).</span><span class="sxs-lookup"><span data-stu-id="5bd85-137">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="5bd85-138">Oprogramowanie pośredniczące lokalizacji odczytuje plik cookie na kolejnych żądaniach, aby ustawić kulturę użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5bd85-138">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="5bd85-139">Użycie pliku cookie zapewnia, że połączenie z użyciem protokołu WebSocket może prawidłowo propagować kulturę.</span><span class="sxs-lookup"><span data-stu-id="5bd85-139">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="5bd85-140">Jeśli schematy lokalizacji są oparte na ścieżce URL lub ciągu zapytania, schemat może nie być w stanie współdziałać z usługą WebSockets, więc nie będzie można zachować kultury.</span><span class="sxs-lookup"><span data-stu-id="5bd85-140">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="5bd85-141">W związku z tym zalecanym podejściem jest użycie pliku cookie kultury lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="5bd85-141">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="5bd85-142">Każda technika może służyć do przypisywania kultury, jeśli kultura jest utrwalona w pliku cookie lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="5bd85-142">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="5bd85-143">Jeśli aplikacja ma już ustalony schemat lokalizacji dla ASP.NET Core po stronie serwera, Kontynuuj korzystanie z istniejącej infrastruktury lokalizacji aplikacji i Ustaw plik cookie kultury lokalizacji w schemacie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5bd85-143">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="5bd85-144">Poniższy przykład pokazuje, jak ustawić bieżącą kulturę w pliku cookie, który może zostać odczytany przez oprogramowanie pośredniczące lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="5bd85-144">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="5bd85-145">Utwórz plik *Pages/hosta. cshtml. cs* z następującą zawartością w aplikacji Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="5bd85-145">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

<span data-ttu-id="5bd85-146">Lokalizacja jest obsługiwana przez aplikację w następującej kolejności zdarzeń:</span><span class="sxs-lookup"><span data-stu-id="5bd85-146">Localization is handled by the app in the following sequence of events:</span></span>

1. <span data-ttu-id="5bd85-147">Przeglądarka wysyła początkowe żądanie HTTP do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5bd85-147">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="5bd85-148">Kultura jest przypisana przez oprogramowanie pośredniczące lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="5bd85-148">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="5bd85-149">Metoda `OnGet` w *_Host. cshtml. cs* utrzymuje kulturę w pliku cookie jako część odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="5bd85-149">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="5bd85-150">Przeglądarka otwiera połączenie WebSocket, aby utworzyć interakcyjną sesję serwera Blazor.</span><span class="sxs-lookup"><span data-stu-id="5bd85-150">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="5bd85-151">Oprogramowanie pośredniczące lokalizacji odczytuje plik cookie i przypisuje kulturę.</span><span class="sxs-lookup"><span data-stu-id="5bd85-151">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="5bd85-152">Sesja serwera Blazor rozpoczyna się od poprawnej kultury.</span><span class="sxs-lookup"><span data-stu-id="5bd85-152">The Blazor Server session begins with the correct culture.</span></span>

### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="5bd85-153">Podaj interfejs użytkownika, aby wybrać kulturę</span><span class="sxs-lookup"><span data-stu-id="5bd85-153">Provide UI to choose the culture</span></span>

<span data-ttu-id="5bd85-154">Aby zapewnić interfejs użytkownika, aby umożliwić użytkownikowi wybranie kultury, zalecane jest *podejście oparte na przekierowaniu* .</span><span class="sxs-lookup"><span data-stu-id="5bd85-154">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="5bd85-155">Ten proces jest podobny do tego, co się dzieje w aplikacji sieci Web, gdy użytkownik próbuje uzyskać dostęp do bezpiecznego zasobu&mdash;użytkownik zostanie przekierowany do strony logowania, a następnie przekierowany z powrotem do oryginalnego zasobu.</span><span class="sxs-lookup"><span data-stu-id="5bd85-155">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="5bd85-156">Aplikacja utrzymuje wybraną kulturę użytkownika za pośrednictwem przekierowania do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5bd85-156">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="5bd85-157">Kontroler ustawia wybraną kulturę użytkownika na plik cookie i przekierowuje użytkownika z powrotem do oryginalnego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="5bd85-157">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="5bd85-158">Ustanów punkt końcowy HTTP na serwerze, aby ustawić kulturę wybraną przez użytkownika w pliku cookie i wykonać przekierowanie z powrotem do oryginalnego identyfikatora URI:</span><span class="sxs-lookup"><span data-stu-id="5bd85-158">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> <span data-ttu-id="5bd85-159">Użyj wyniku działania `LocalRedirect`, aby zapobiec atakom typu "Open redirect".</span><span class="sxs-lookup"><span data-stu-id="5bd85-159">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="5bd85-160">Aby uzyskać więcej informacji, zobacz <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="5bd85-160">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="5bd85-161">Poniższy składnik przedstawia przykład sposobu wykonywania wstępnego przekierowania, gdy użytkownik wybierze kulturę:</span><span class="sxs-lookup"><span data-stu-id="5bd85-161">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```razor
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="5bd85-162">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="5bd85-162">Additional resources</span></span>

* <xref:fundamentals/localization>
