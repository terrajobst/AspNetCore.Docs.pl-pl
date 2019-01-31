---
title: Użyj interfejsu API sieci web Konwencji
author: pranavkm
description: Dowiedz się więcej na temat Konwencji interfejsu API sieci web w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 5ae96b213a19464045e1d0b1a76f8eb81089dc5b
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236344"
---
# <a name="use-web-api-conventions"></a>Użyj interfejsu API sieci web Konwencji

Przez [Pranav Krishnamoorthy](https://github.com/pranavkm) i [Scott Addie](https://github.com/scottaddie)

ASP.NET Core 2,2 lub nowszym zawiera sposób wyodrębniania typowe [dokumentacji interfejsu API](xref:tutorials/web-api-help-pages-using-swagger) i zastosować je do wielu akcji kontrolerów i wszystkich kontrolerów w zestawie. Konwencje interfejsu API sieci Web są stanowi zastępstwa dla dekoracji poszczególne akcje za pomocą [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).

Konwencję umożliwia:

* Zdefiniuj najczęściej używane typy zwracane i kodów stanu zwrócony z określonego typu działania.
* Zidentyfikuj akcji odbiegających od zdefiniowany standard.

Platforma ASP.NET Core MVC 2,2 lub nowszym zawiera zestaw domyślnych Konwencji w `Microsoft.AspNetCore.Mvc.DefaultApiConventions`. Konwencje są oparte na kontrolerze (*ValuesController.cs*) podany w programie ASP.NET Core **API** szablonu projektu. Jeśli Twoje działania wzorców w szablonie, powinno być pomyślnego używania domyślnych Konwencji. Jeśli domyślnych Konwencji nie odpowiada Twoim potrzebom, zobacz [utworzyć internetowy interfejs API konwencje](#create-web-api-conventions).

W czasie wykonywania <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> rozumie Konwencji. `ApiExplorer` to Abstrakcja MVC do komunikowania się z [OpenAPI](https://www.openapis.org/) (Swagger) generatorów dokumentu. Atrybuty z Konwencji stosowane są skojarzone z akcją i znajdują się w dokumentacji interfejsu OpenAPI akcji. [Interfejs API analizatorów](xref:web-api/advanced/analyzers) również zrozumienie Konwencji. Jeśli Twoja akcja jest nietypowe (na przykład zwraca kod stanu, który nie jest udokumentowany zgodnie z Konwencją stosowane), ostrzeżenie zachęca do dokumentowania kodu stanu.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="apply-web-api-conventions"></a>Zastosuj konwencje interfejsu API sieci web

Konwencje nie tworzą; Każde działanie może być skojarzony z dokładnie jednego Konwencji. Bardziej szczegółowe konwencje mają pierwszeństwo przed mniej określonych konwencji. Zaznaczenie jest deterministyczna, gdy co najmniej dwóch Konwencji ten sam priorytet zastosować do akcji. Istnieją następujące opcje, aby zastosować Konwencję do działania z najbardziej specyficznych do najmniej specyficznych:

1. `Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Ma zastosowanie do poszczególnych działań i określa typ Konwencji i metody Konwencji, która ma zastosowanie.

    W poniższym przykładzie, domyślny typ Konwencji firmy `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` metoda Konwencji jest stosowana do `Update` akcji:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionMethod&highlight=3)]

    `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` Metoda Konwencji stosuje się następujące atrybuty do akcji:

    ```csharp
    [ProducesDefaultResponseType]
    [ProducesResponseType(204)]
    [ProducesResponseType(404)]
    [ProducesResponseType(400)]
    ```

Aby uzyskać więcej informacji na temat `[ProducesDefaultResponseType]`, zobacz [odpowiedź domyślna](https://swagger.io/docs/specification/describing-responses/#default).

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` stosowane do kontrolera &mdash; dotyczy typ określonej Konwencji wszystkich akcji w kontrolerze. Metoda Konwencji zostanie nadany wskazówek, które określają akcje, do których zostanie zastosowana metoda Konwencji. Aby uzyskać więcej informacji na temat wskazówek dotyczących serwerów, zobacz [utworzyć internetowy interfejs API konwencje](#create-web-api-conventions)).

    W poniższym przykładzie na wybranie domyślnego zestawu Konwencji jest stosowana do wszystkich akcji w *ContactsConventionController*:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionTypeAttribute&highlight=2)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` zastosowany do zestawu &mdash; zastosowanie Konwencji określony typ do wszystkich kontrolerów w bieżącym zestawie. Jako zalecenie, Zastosuj atrybuty poziomu zestawu w *Startup.cs* pliku.

    W poniższym przykładzie na wybranie domyślnego zestawu Konwencji jest stosowana do wszystkich kontrolerów w zestawie:

    [!code-csharp[](conventions/sample/Startup.cs?name=snippet_ApiConventionTypeAttribute&highlight=1)]

## <a name="create-web-api-conventions"></a>Tworzenie konwencje interfejsu API sieci web

Jeśli domyślnych Konwencji interfejsu API nie odpowiada Twoim potrzebom, tworzenie własnych Konwencji odpowiadającym. Konwencja to:

* Typu statycznego za pomocą metod.
* Możliwość definiowania [typów odpowiedzi](#response-types) i [wymaganiami w zakresie nazewnictwa](#naming-requirements) na akcje.

### <a name="response-types"></a>Typy odpowiedzi

Te metody są oznaczony za pomocą `[ProducesResponseType]` lub `[ProducesDefaultResponseType]` atrybutów. Na przykład:

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

W przypadku nieobecności dokładniejszą atrybuty metadanych na stosowanie niniejszej Konwencji, do zestawu wymusza który:

* Metoda Konwencji odnosi się do dowolnej akcji o nazwie `Find`.
* Parametr o nazwie `id` znajduje się na `Find` akcji.

### <a name="naming-requirements"></a>Wymagania dotyczące nazewnictwa

`[ApiConventionNameMatch]` i `[ApiConventionTypeMatch]` atrybuty mogą być stosowane do metody Konwencji, która określa akcje, których dotyczą. Na przykład:

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

W poprzednim przykładzie:

* `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` Opcja zastosowany do metody wskazuje, że Konwencji pasuje do żadnych działań z prefiksem "Znajdź". Przykłady działań pasujących `Find`, `FindPet`, i `FindById`.
* `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` Zastosowany do parametru wskazuje Konwencji zgodność metody przy użyciu dokładnie jeden parametr końcówce identyfikatora sufiksu. Przykłady obejmują parametry `id` lub `petId`. `ApiConventionTypeMatch` można podobnie można zastosować do typów ograniczenie z typem parametru. A `params[]` argument wskazuje pozostałe parametry, których nie trzeba jawnie można dopasować.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:web-api/advanced/analyzers>
* <xref:tutorials/web-api-help-pages-using-swagger>
