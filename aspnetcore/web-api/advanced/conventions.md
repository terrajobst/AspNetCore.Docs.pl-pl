---
title: Użyj interfejsu API sieci web Konwencji
author: pranavkm
description: Dowiedz się więcej na temat Konwencji interfejsu API sieci web w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: ede9a46c160cf6a49aa93da710af0bf0b8f59acc
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710078"
---
# <a name="use-web-api-conventions"></a>Użyj interfejsu API sieci web Konwencji

ASP.NET Core 2.2 wprowadza sposób wyodrębniania typowe [dokumentacji interfejsu API](xref:tutorials/web-api-help-pages-using-swagger) i zastosować je do wielu akcji kontrolerów i wszystkich kontrolerów w zestawie. Konwencje interfejsu API sieci Web są stanowi zastępstwa dla dekoracji poszczególne akcje za pomocą [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute). Umożliwia definiowanie najczęściej "konwencjonalne" typy zwracane i kodami stanu, które zwracają z akcję przy użyciu sposób na wybór metody Konwencji, która ma zastosowanie do akcji.

Domyślnie program ASP.NET Core MVC 2.2 jest dostarczany z zestawem domyślnych Konwencji `Microsoft.AspNetCore.Mvc.DefaultApiConventions`. Konwencje są oparte na kontrolerze, który szkielety mechanizmów platformy ASP.NET Core. Jeśli Twoje działania oparte na wzorcu tworzenia szkieletów generuje, należy pomyślnego używania domyślnych Konwencji.

W czasie wykonywania <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> rozumie Konwencji. `ApiExplorer` to Abstrakcja MVC do komunikowania się z generatorów dokumencie interfejsu Open API. Atrybuty z stosowanych Konwencji z akcji i znajdują się w dokumentacji programu Swagger akcji. Interfejs API analizatorów zrozumieć również Konwencji. Jeśli Twoja akcja jest nietypowe (na przykład zwraca kod stanu, który nie jest udokumentowany zgodnie z Konwencją stosowane), generuje ostrzeżenie zachęcanie do dokumentu go.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="apply-web-api-conventions"></a>Zastosuj konwencje interfejsu API sieci web

Istnieją trzy sposoby, aby zastosować Konwencję. Konwencje nie tworzą. Każde działanie może być skojarzony z dokładnie jednego Konwencji. Bardziej szczegółowe konwencje (szczegóły przedstawiono poniżej) mają pierwszeństwo przed mniej określonych zadań. Zaznaczenie jest deterministyczna, gdy co najmniej dwóch Konwencji ten sam priorytet zastosować do akcji. Istnieją następujące opcje, aby zastosować Konwencję do działania z najbardziej specyficznych do najmniej specyficznych:

1. `Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Ma zastosowanie do poszczególnych działań i określa typ Konwencji i metody Konwencji, która ma zastosowanie. W poniższym przykładzie metoda Konwencji `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` jest stosowany do `Update` akcji:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` stosowane do kontrolera &mdash; dotyczy typ Konwencji wszystkich akcji w kontrolerze. Konwencja metody są oznaczone za pomocą wskazówek, które określają, jakie akcje będą dotyczyć (szczegóły w ramach tworzenia Konwencji). Na przykład:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` zastosowany do zestawu &mdash; stosuje typu Konwencji do wszystkich kontrolerów w bieżącym zestawie. Na przykład:

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a>Tworzenie konwencje interfejsu API sieci web

Konwencja jest typu statycznego za pomocą metod. Te metody są oznaczony za pomocą `[ProducesResponseType]` lub `[ProducesDefaultResponseType]` atrybutów.

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

Stosując tę Konwencję do zestawu wyników w metodzie Konwencji, stosując się do dowolnej akcji o nazwie `Find` i o dokładnie jeden parametr o nazwie `id`, tak długo, jak nie mają inne dokładniejszą atrybuty metadanych.

Oprócz `[ProducesResponseType]` i `[ProducesDefaultResponseType]`, `[ApiConventionNameMatch]` i `[ApiConventionTypeMatch]` mogą być stosowane do metody Konwencji, która określa te metody, które odnoszą się do. Na przykład:

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` Zastosowany do metody, opcja wskazuje Konwencji może dopasować dowolną akcję tak długo, jak jest prefiksem "Znajdź". Obejmuje to metody takie jak `Find`, `FindPet`, lub `FindById`.
* `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` Zastosowany do parametru wskazuje Konwencji można dopasować metody przy użyciu dokładnie jeden parametr końcówce identyfikatora sufiksu. Obejmuje to parametry takie jak `id` lub `petId`. `ApiConventionTypeMatch` można podobnie można zastosować do typów w celu ograniczenia typu parametru. A `params[]` argument może służyć do wskazania pozostałe parametry, których nie trzeba jawnie można dopasować.
