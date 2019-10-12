---
title: Rozszerzalność lokalizacji
author: hishamco
description: Dowiedz się, jak rozłożyć interfejsy API lokalizacji w aplikacjach ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/03/2019
uid: fundamentals/localization-extensibility
ms.openlocfilehash: dfa2efe78b2e1e118e6b3f09bfc41f3330e1d721
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2019
ms.locfileid: "72288941"
---
# <a name="localization-extensibility"></a>Rozszerzalność lokalizacji

Według [Hisham bin Ateya](https://github.com/hishamco)

W tym artykule:

* Wyświetla listę punktów rozszerzalności w interfejsach API lokalizacji.
* Zawiera instrukcje dotyczące sposobu rozbudowania lokalizacji aplikacji ASP.NET Core.

## <a name="extensible-points-in-localization-apis"></a>Rozszerzalne punkty w interfejsach API lokalizacji

Interfejsy API lokalizacji ASP.NET Core są kompilowane do rozszerzalności. Rozszerzalność umożliwia deweloperom dostosowanie lokalizacji zgodnie z ich potrzebami. Na przykład [OrchardCore](https://github.com/orchardCMS/OrchardCore/) ma `POStringLocalizer`. `POStringLocalizer` opisano szczegółowo przy użyciu [lokalizacji obiektu przenośnego](xref:fundamentals/portable-object-localization) do przechowywania zasobów lokalizacji za pomocą plików `PO`.

W tym artykule wymieniono dwa główne punkty rozszerzalności zapewniane przez interfejsy API lokalizacji: 

* <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider>
* <xref:Microsoft.Extensions.Localization.IStringLocalizer>

## <a name="localization-culture-providers"></a>Dostawcy kultury lokalizacji

Interfejsy API lokalizacji ASP.NET Core mają czterech dostawców domyślnych, którzy mogą określić bieżącą kulturę żądania wykonania:

* <xref:Microsoft.AspNetCore.Localization.QueryStringRequestCultureProvider>
* <xref:Microsoft.AspNetCore.Localization.CookieRequestCultureProvider>
* <xref:Microsoft.AspNetCore.Localization.AcceptLanguageHeaderRequestCultureProvider>
* <xref:Microsoft.AspNetCore.Localization.CustomRequestCultureProvider>

Poprzedni dostawcy są szczegółowo opisane w dokumentacji [oprogramowania pośredniczącego](xref:fundamentals/localization) . Jeśli dostawcy domyślnie nie spełnią Twoich potrzeb, należy utworzyć niestandardowego dostawcę przy użyciu jednej z następujących metod:

### <a name="use-customrequestcultureprovider"></a>Użyj CustomRequestCultureProvider

<xref:Microsoft.AspNetCore.Localization.CustomRequestCultureProvider> zawiera niestandardowy <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider>, który używa prostego delegata do określenia bieżącej kultury lokalizacji:

::: moniker range="< aspnetcore-3.0"
```csharp
options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
{
    var currentCulture = "en";
    var segments = context.Request.Path.Value.Split(new char[] { '/' }, 
        StringSplitOptions.RemoveEmptyEntries);

    if (segments.Length > 1 && segments[0].Length == 2)
    {
        currentCulture = segments[0];
    }

    var requestCulture = new ProviderCultureResult(culture);
    
    return Task.FromResult(requestCulture);
}));
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"
```csharp
options.AddInitialRequestCultureProvider(new CustomRequestCultureProvider(async context =>
{
    var currentCulture = "en";
    var segments = context.Request.Path.Value.Split(new char[] { '/' }, 
        StringSplitOptions.RemoveEmptyEntries);

    if (segments.Length > 1 && segments[0].Length == 2)
    {
        currentCulture = segments[0];
    }

    var requestCulture = new ProviderCultureResult(culture);
    
    return Task.FromResult(requestCulture);
}));
```

::: moniker-end

### <a name="use-a-new-implemetation-of-requestcultureprovider"></a>Użyj nowego Jawna implementacja RequestCultureProvider

Można utworzyć nową implementację <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider>, która określa informacje o kulturze żądania ze źródła niestandardowego. Na przykład źródło niestandardowe może być plikiem konfiguracji lub bazą danych.

Poniższy przykład pokazuje `AppSettingsRequestCultureProvider`, który rozszerza <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider>, aby określić informacje o kulturze żądania z pliku *appSettings. JSON*:

```csharp
public class AppSettingsRequestCultureProvider : RequestCultureProvider
{
    public string CultureKey { get; set; } = "culture";

    public string UICultureKey { get; set; } = "ui-culture";

    public override Task<ProviderCultureResult> DetermineProviderCultureResult(HttpContext httpContext)
    {
        if (httpContext == null)
        {
            throw new ArgumentNullException();
        }

        var configuration = httpContext.RequestServices.GetService<IConfigurationRoot>();
        var culture = configuration[CultureKey];
        var uiCulture = configuration[UICultureKey];

        if (culture == null && uiCulture == null)
        {
            return Task.FromResult((ProviderCultureResult)null);
        }

        if (culture != null && uiCulture == null)
        {
            uiCulture = culture;
        }

        if (culture == null && uiCulture != null)
        {
            culture = uiCulture;
        }
        
        var providerResultCulture = new ProviderCultureResult(culture, uiCulture);

        return Task.FromResult(providerResultCulture);
    }
}
```

## <a name="localization-resources"></a>Zasoby lokalizacji

Lokalizacja ASP.NET Core zapewnia <xref:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer>. <xref:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer> to implementacja <xref:Microsoft.Extensions.Localization.IStringLocalizer>, która używa `resx` do przechowywania zasobów lokalizacji.

Nie można używać plików `resx`. Implementując `IStringLocalized`, można użyć dowolnego źródła danych.

Następujące przykładowe projekty implementują <xref:Microsoft.Extensions.Localization.IStringLocalizer>: 

* [EFStringLocalizer](https://github.com/aspnet/Entropy/tree/master/samples/Localization.EntityFramework)
* [JsonStringLocalizer](https://github.com/hishamco/My.Extensions.Localization.Json)
* [Sqllokalizowaer](https://github.com/damienbod/AspNetCoreLocalization)
