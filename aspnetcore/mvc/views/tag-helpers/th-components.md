---
title: Oznacz składniki pomocnika w ASP.NET Core
author: scottaddie
description: Informacje o składnikach pomocników tagów i sposobach ich użycia w ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.date: 06/12/2019
uid: mvc/views/tag-helpers/th-components
ms.openlocfilehash: 23e244649350b41e4112d10df63139864e5b4381
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022219"
---
# <a name="tag-helper-components-in-aspnet-core"></a>Oznacz składniki pomocnika w ASP.NET Core

Przez [Scott Addie](https://twitter.com/Scott_Addie) i [Fiyaz bin Hasan](https://github.com/fiyazbinhasan)

Składnik pomocnika tagów to pomocnik tagów, który umożliwia warunkowe modyfikowanie lub Dodawanie elementów HTML z kodu po stronie serwera. Ta funkcja jest dostępna w ASP.NET Core 2,0 lub nowszej.

ASP.NET Core obejmuje dwa wbudowane składniki pomocnika tagów: `head` i. `body` Znajdują się one w <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers> przestrzeni nazw i mogą być używane zarówno w MVC, jak i Razor Pages. Składniki pomocnika tagów nie wymagają rejestracji w aplikacji *_ViewImports. cshtml*.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/th-components/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="use-cases"></a>Przypadki zastosowań

Dwa typowe przypadki użycia składników pomocnika tagów obejmują:

1. [`<link>` Wprowadzanie`<head>`do.](#inject-into-html-head-element)
1. [`<script>` Wprowadzanie`<body>`do.](#inject-into-html-body-element)

W poniższych sekcjach opisano te przypadki użycia.

### <a name="inject-into-html-head-element"></a>Wsuń do elementu głównego HTML

Wewnątrz elementu HTML `<head>` pliki CSS są zwykle importowane z elementem HTML. `<link>` Poniższy kod `<link>` `head` wprowadza element do elementu przy użyciu składnika pomocnika tagów: `<head>`

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressStyleTagHelperComponent.cs)]

W powyższym kodzie:

* `AddressStyleTagHelperComponent`implementuje <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent>. Streszczenie:
  * Umożliwia inicjowanie klasy z <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext>.
  * Umożliwia używanie składników pomocnika tagów do dodawania lub modyfikowania elementów HTML.
* <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.Order*> Właściwość definiuje kolejność, w jakiej składniki są renderowane. `Order`jest konieczne, gdy w aplikacji istnieje wiele użycia składników pomocnika tagów.
* <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperComponent.ProcessAsync*>porównuje wartość <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContext.TagName*> właściwości kontekstu wykonania z `head`. Jeśli wynikiem porównania jest wartość true, zawartość `_style` pola jest wstrzykiwana do elementu HTML. `<head>`

### <a name="inject-into-html-body-element"></a>Wsuń do elementu treści HTML

Składnik pomocnika `<script>`tagówmoże wstrzyknąć element do `<body>` elementu. `body` Poniższy kod ilustruje tę technikę:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressScriptTagHelperComponent.cs)]

Do przechowywania `<script>` elementu używany jest oddzielny plik HTML. Plik HTML sprawia, że kod jest bardziej przejrzysty i bardziej czytelny. Poprzedni kod odczytuje zawartość *TagHelpers/templates/AddressToolTipScript.html* i dołącza ją z danymi wyjściowymi pomocnika tagów. Plik *AddressToolTipScript. html* zawiera następujące znaczniki:

[!code-html[](th-components/samples/RazorPagesSample/TagHelpers/Templates/AddressToolTipScript.html)]

Poprzedni kod tworzy `<address>` `printable` element [widget etykietki narzędzia Bootstrap](https://getbootstrap.com/docs/3.3/javascript/#tooltips) dla każdego elementu, który zawiera atrybut. Efekt jest widoczny, gdy wskaźnik myszy znajduje się nad elementem.

## <a name="register-a-component"></a>Rejestrowanie składnika

Składnik pomocnika tagów należy dodać do kolekcji składników pomocnika tagów aplikacji. Istnieją trzy sposoby dodawania do kolekcji:

* [Rejestracja za pośrednictwem kontenera usług](#registration-via-services-container)
* [Rejestracja za pośrednictwem pliku Razor](#registration-via-razor-file)
* [Rejestracja za pośrednictwem modelu lub kontrolera strony](#registration-via-page-model-or-controller)

### <a name="registration-via-services-container"></a>Rejestracja za pośrednictwem kontenera usług

Jeśli Klasa składnika pomocnika tagów nie jest zarządzana za pomocą <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.ITagHelperComponentManager>programu, musi być zarejestrowana w systemie wtrysku [zależności (di)](xref:fundamentals/dependency-injection) . Poniższy `Startup.ConfigureServices` kod `AddressScriptTagHelperComponent` rejestruje klasy i z przejściowym okresem istnienia: [](xref:fundamentals/dependency-injection#lifetime-and-registration-options) `AddressStyleTagHelperComponent`

[!code-csharp[](th-components/samples/RazorPagesSample/Startup.cs?name=snippet_ConfigureServices&highlight=12-15)]

### <a name="registration-via-razor-file"></a>Rejestracja za pośrednictwem pliku Razor

Jeśli składnik pomocnika tagów nie jest zarejestrowany przy użyciu DI, może być zarejestrowany na stronie Razor Pages lub widoku MVC. Ta technika służy do kontrolowania wstrzykniętego znacznika i kolejności wykonywania składników z pliku Razor.

`ITagHelperComponentManager`służy do dodawania składników pomocnika tagów lub usuwania ich z aplikacji. Poniższy kod ilustruje tę technikę `AddressTagHelperComponent`:

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_ITagHelperComponentManager)]

W powyższym kodzie:

* `@inject` Dyrektywa zawiera `ITagHelperComponentManager`wystąpienie. Wystąpienie jest przypisane do zmiennej o nazwie `manager` , aby uzyskać dostęp do niej w pliku Razor.
* Wystąpienie `AddressTagHelperComponent` jest dodawane do kolekcji składników pomocnika tagów aplikacji.

`AddressTagHelperComponent`zmodyfikowano, aby pomieścić konstruktora, który akceptuje `markup` parametry `order` i:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_Constructor)]

Podany `markup` parametr jest używany w `ProcessAsync` następujący sposób:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponent.cs?name=snippet_ProcessAsync&highlight=10-11)]

### <a name="registration-via-page-model-or-controller"></a>Rejestracja za pośrednictwem modelu lub kontrolera strony

Jeśli składnik pomocnika tagów nie jest zarejestrowany w programie DI, może być zarejestrowany z poziomu modelu strony Razor Pages lub kontrolera MVC. Ta technika jest przydatna do oddzielania C# logiki od plików Razor.

Iniekcja konstruktora jest używana w celu uzyskania dostępu `ITagHelperComponentManager`do wystąpienia. Składnik pomocnika tagów jest dodawany do kolekcji składników pomocnika tagów wystąpienia. Następujący Razor Pages model strony ilustruje tę technikę `AddressTagHelperComponent`:

[!code-csharp[](th-components/samples/RazorPagesSample/Pages/Index.cshtml.cs?name=snippet_IndexModelClass)]

W powyższym kodzie:

* Iniekcja konstruktora jest używana w celu uzyskania dostępu `ITagHelperComponentManager`do wystąpienia.
* Wystąpienie `AddressTagHelperComponent` jest dodawane do kolekcji składników pomocnika tagów aplikacji.

## <a name="create-a-component"></a>Tworzenie składnika

Aby utworzyć składnik pomocnika tagów niestandardowych:

* Utwórz klasę publiczną wyprowadzaną z <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperComponentTagHelper>.
* Zastosuj atrybut [[HtmlTargetElement]](xref:Microsoft.AspNetCore.Razor.TagHelpers.HtmlTargetElementAttribute) do klasy. Określ nazwę docelowego elementu HTML.
* *Opcjonalne*: Zastosuj atrybut [[EditorBrowsable (EditorBrowsableState. Never)]](xref:System.ComponentModel.EditorBrowsableAttribute) do klasy, aby pominąć wyświetlanie typu w IntelliSense.

Poniższy kod tworzy składnik pomocnika tagów niestandardowych, który jest przeznaczony `<address>` dla elementu HTML:

[!code-csharp[](th-components/samples/RazorPagesSample/TagHelpers/AddressTagHelperComponentTagHelper.cs)]

Użyj składnika pomocnika tagów niestandardowych `address` , aby wstrzyknąć znacznik HTML w następujący sposób:

```csharp
public class AddressTagHelperComponent : TagHelperComponent
{
    private readonly string _printableButton =
        "<button type='button' class='btn btn-info' onclick=\"window.open("
        "'https://binged.it/2AXRRYw')\">" +
        "<span class='glyphicon glyphicon-road' aria-hidden='true'></span>" +
        "</button>";

    public override int Order => 3;

    public override async Task ProcessAsync(TagHelperContext context,
                                            TagHelperOutput output)
    {
        if (string.Equals(context.TagName, "address",
                StringComparison.OrdinalIgnoreCase) &&
            output.Attributes.ContainsName("printable"))
        {
            var content = await output.GetChildContentAsync();
            output.Content.SetHtmlContent(
                $"<div>{content.GetContent()}</div>{_printableButton}");
        }
    }
}
```

Poprzednia `ProcessAsync` metoda wprowadza kod HTML udostępniony do <xref:Microsoft.AspNetCore.Razor.TagHelpers.TagHelperContent.SetHtmlContent*> zgodnego `<address>` elementu. Iniekcja występuje, gdy:

* Wartość `TagName` właściwości kontekstu wykonania jest równa `address`.
* Odpowiadający `<address>` element `printable` ma atrybut.

Na przykład, `if` instrukcja daje w wyniku wartość true podczas przetwarzania następującego `<address>` elementu:

[!code-cshtml[](th-components/samples/RazorPagesSample/Pages/Contact.cshtml?name=snippet_AddressPrintable)]

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
* <xref:mvc/views/tag-helpers/builtin-th/Index>
