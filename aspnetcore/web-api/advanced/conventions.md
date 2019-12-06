---
title: Korzystanie z Konwencji interfejsu API sieci Web
author: pranavkm
description: Dowiedz się więcej na temat Konwencji interfejsu API sieci Web w ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: web-api/advanced/conventions
ms.openlocfilehash: 2c7e33da24322504fc5e1be83c0b814710186687
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881318"
---
# <a name="use-web-api-conventions"></a>Korzystanie z Konwencji interfejsu API sieci Web

Autorzy [Pranav Krishnamoorthy](https://github.com/pranavkm) i [Scott Addie](https://github.com/scottaddie)

ASP.NET Core 2,2 i nowsze zawierają sposób wyodrębnienia typowej [dokumentacji interfejsu API](xref:tutorials/web-api-help-pages-using-swagger) i zastosowania jej do wielu akcji, kontrolerów lub wszystkich kontrolerów w ramach zestawu. Konwencje interfejsu API sieci Web stanowią podstawę do dekorowania nazwy poszczególnych akcji z [`[ProducesResponseType]`](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).

Konwencja umożliwia:

* Zdefiniuj najpopularniejsze typy zwracane i kody stanu zwrócone przez określony typ akcji.
* Zidentyfikuj akcje odbiegające od zdefiniowanego standardu.

ASP.NET Core MVC 2,2 i nowsze zawierają zestaw domyślnych Konwencji w <xref:Microsoft.AspNetCore.Mvc.DefaultApiConventions?displayProperty=fullName>. Konwencje są oparte na kontrolerze (*ValuesController.cs*), który znajduje się w szablonie projektu **interfejsu API** ASP.NET Core. Jeśli akcje są zgodne ze wzorcami w szablonie, należy pomyślnie korzystać z domyślnych Konwencji. Jeśli domyślne konwencje nie spełniają Twoich potrzeb, zobacz [Tworzenie Konwencji internetowego interfejsu API](#create-web-api-conventions).

W czasie wykonywania program <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> rozumie konwencje. `ApiExplorer` to Abstrakcja MVC do komunikowania się z [openapi](https://www.openapis.org/) (znanego również jako Swagger) generatorami dokumentów. Atrybuty z stosowanej Konwencji są skojarzone z akcją i są zawarte w dokumentacji OpenAPI akcji. [Analizatory interfejsów API](xref:web-api/advanced/analyzers) są również zrozumiałe Konwencji. Jeśli akcja jest niekonwencjonalny (na przykład zwraca kod stanu, który nie jest udokumentowany przez zastosowana Konwencja), ostrzeżenie zachęca do dokumentowania kodu stanu.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="apply-web-api-conventions"></a>Zastosuj konwencje internetowego interfejsu API

Konwencje nie tworzą; Każda akcja może być skojarzona z dokładnie jedną Konwencją. Bardziej szczegółowe konwencje mają pierwszeństwo przed określonymi konwencjami. Zaznaczenie jest niedeterministyczne, jeśli co najmniej dwie konwencje o takim samym priorytecie mają zastosowanie do akcji. Następujące opcje są stosowane w celu zastosowania konwencji do akcji, od najbardziej do najmniej określonych:

1. `Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; ma zastosowanie do poszczególnych akcji i określa typ Konwencji oraz metodę Konwencji, która ma zastosowanie.

    W poniższym przykładzie metoda `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` Konwencji domyślnego typu Konwencji jest stosowana do akcji `Update`:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionMethod&highlight=3)]

    Metoda Konwencji `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` stosuje następujące atrybuty do akcji:

    ```csharp
    [ProducesDefaultResponseType]
    [ProducesResponseType(StatusCodes.Status204NoContent)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    [ProducesResponseType(StatusCodes.Status400BadRequest)]
    ```

    Aby uzyskać więcej informacji na temat `[ProducesDefaultResponseType]`, zobacz [odpowiedź domyślna](https://swagger.io/docs/specification/describing-responses/#default).

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` zastosowana do kontrolera &mdash; stosuje określony typ Konwencji do wszystkich akcji na kontrolerze. Metoda Konwencji jest oznaczona przy użyciu wskazówek, które określają akcje, których dotyczy Metoda Konwencji. Aby uzyskać więcej informacji na temat wskazówek, zobacz [Tworzenie Konwencji interfejsu API sieci Web](#create-web-api-conventions)).

    W poniższym przykładzie domyślny zestaw Konwencji jest stosowany do wszystkich akcji w *ContactsConventionController*:

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionTypeAttribute&highlight=2)]

1. `Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` stosowany do zestawu &mdash; stosuje określony typ Konwencji do wszystkich kontrolerów w bieżącym zestawie. Zgodnie z zaleceniem Zastosuj atrybuty na poziomie zestawu w pliku *Startup.cs* .

    W poniższym przykładzie domyślny zestaw Konwencji jest stosowany do wszystkich kontrolerów w zestawie:

    [!code-csharp[](conventions/sample/Startup.cs?name=snippet_ApiConventionTypeAttribute&highlight=1)]

## <a name="create-web-api-conventions"></a>Tworzenie Konwencji interfejsu API sieci Web

Jeśli domyślne konwencje interfejsów API nie spełniają Twoich potrzeb, należy utworzyć własne konwencje. Konwencja:

* Typ statyczny z metodami.
* Możliwość definiowania [typów odpowiedzi](#response-types) i [wymagań dotyczących nazewnictwa](#naming-requirements) w akcjach.

### <a name="response-types"></a>Typy odpowiedzi

Te metody są opatrzone adnotacją `[ProducesResponseType]` lub `[ProducesDefaultResponseType]` atrybutów. Na przykład:

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(StatusCodes.Status200OK)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    public static void Find(int id)
    {
    }
}
```

W przypadku braku szczegółowych atrybutów metadanych zastosowanie tej Konwencji do zestawu wymusza, że:

* Metoda Konwencji ma zastosowanie do dowolnej akcji o nazwie `Find`.
* W akcji `Find` występuje parametr o nazwie `id`.

### <a name="naming-requirements"></a>Wymagania dotyczące nazewnictwa

Atrybuty `[ApiConventionNameMatch]` i `[ApiConventionTypeMatch]` mogą być stosowane do metody Konwencji, która określa akcje, do których mają zastosowanie. Na przykład:

```csharp
[ProducesResponseType(StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status404NotFound)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

W poprzednim przykładzie:

* Opcja `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` stosowana do metody wskazuje, że Konwencja pasuje do każdej akcji poprzedzonej prefiksem "Find". Przykłady pasujących akcji obejmują `Find`, `FindPet`i `FindById`.
* `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` zastosowana do parametru wskazuje, że Konwencja dopasowuje metody z dokładnie jednym parametrem kończącym się identyfikatorem sufiksu. Przykłady obejmują parametry, takie jak `id` lub `petId`. `ApiConventionTypeMatch` można w podobny sposób zastosować do typów, aby ograniczyć typ parametru. Argument `params[]` wskazuje pozostałe parametry, które nie muszą być jawnie dopasowane.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:web-api/advanced/analyzers>
* <xref:tutorials/web-api-help-pages-using-swagger>
